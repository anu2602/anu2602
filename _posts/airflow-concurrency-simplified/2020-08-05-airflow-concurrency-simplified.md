---
title: Airflow Concurrency Configuration Simplified!
date: 2020-08-05 11:58:47 +05:30
tags: [Airflow, Prod]
description: 
image: "/airflow-concurrency-simplified/airflow.jpeg"
comments: true
---

Anyone who goes beyond Airflow PoC and starts to build production grade data pipelines with Airflow would rquire to configure concurrent task execution. Unfortunately the documentation around it is scarce and to complicate the matter the configuration parameters are named in a very <a href="https://issues.apache.org/jira/browse/AIRFLOW-57" target="_blank" >unintuitive and confusing way</a>. 

<figure>
<img src="confused.jpg" alt="confused">
<figcaption style="color: grey !important;"> 
	Photo by <a href="https://unsplash.com/@benwhitephotography" style="color: grey !important;" target="_blank">Ben White </a> 
</figcaption>
</figure>

Though I can't change the name of the properties, I have made an attempt to decode the names. Hoefully it helps some of you, so that you can focus on more pressing problems in your data pipeline. 

##### `[core]`
**`parallelism`** [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#parallelism){:target="_blank"} ]: Maximum number of task instances that can run concurrently on Airflow installation, i.e. the maximum number of task instances that can run in the entire airflow cluster. This is the only setting that applies to entire cluster as opposed to the given node. If you have two host running Airflow worker, combined total of task running on both workers will not exceed this value. You can also think of this is as maximum number of taks that will have state='running' in airflow metadata db. This value should be configured same on all the worker nodes of a given Airflow Cluster. The default value of `32`. If you are using `CeleryExecutor`, this should be sum of `worker_concurrency` for all nodes.


**`dag_concurrency`** [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#dag-concurrency){:target="_blank"} ]: This parameter determines how many task instances Airflow scheduler is able to schedule concurrently per DAG. This can be thought of as maximum tasks that can be scheduled concurrently, per DAG. Since schedular is global for cluster this property is also applicable to Airflow Cluster/installation as one entity. You should keep this value same on all the worker nodes of your Airflow Cluster. 
This parameter can be overwritten at DAG level by: ```dag = DAG('example2', concurrency=10)```


**`max_active_runs_per_dag`** [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#max_active_runs_per_dag){:target="_blank"} ]: This paramenter is important but does not really controls the concurrency directly, instead it specifies how many total runs of a DAG are allowed to run for the Airflow cluster. This helps in controling the fair allocation of resources and making sure that one DAG is not taking up all the resource. This parameter can be overwritten at DAG level by: ```dag = DAG('my_dag_id', max_active_runs=1) ```


##### `[scheduler]`
**`max_threads`** [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#max_threads){:target="_blank"} ]:The scheduler can run multiple threads in parallel to schedule dags. This parameter specifies how many threads will run scheduler run. Adjust this number based on CPU resources available - the higher the value, the more resources you'll need. Update: setting [has been renamed](https://github.com/apache/airflow/pull/12605){:target="_blank"} to `parsing_processes` in airflow 2.0. 


##### `[celery]`

**`worker_concurrency`** [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#worker-concurrency){:target="_blank"} ]: This is the number of  celery workers, per Airflow Worker. You would typically run one Airflow worker per Airflow node. So this can be considerd as per Airflow node version of `parallelism` setting. This is maximum number of task instances that can on a given Airflow worker can execute. You can have different settings for different worker node depending on number of available CPU Cores. This parameter has default value of `16`.


**`pool`** [ [Ref ](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#pool){:target="_blank"} ]:  
 This is type of celery pool implementation to be used and shold not be confused with [Airflow Pool](https://airflow.apache.org/docs/apache-airflow/stable/concepts.html#pools){:target="_blank"}, which I will cover briefly later in this article. For this settings options are: prefork (default), eventlet, gevent or solo. For usual data pipelines, prefork works fine. If you have many I/O blocking tasks in your data pipelines, it is worth exploring gevent or eventlet. Please refer to [this post](https://www.distributedpython.com/2018/10/26/celery-execution-pool/){:target="_blank"} on distributed python for more details on various options.

**`sync_parallelism`** [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#parallelism){:target="_blank"} ]: This is number of processes CeleryExecutor uses to sync task state. Value 0 implies to use max(1, number of cores - 1) processes.


**`worker_autoscale`** [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#worker-autoscale){:target="_blank"} ]: The maximum and minimum celery workers that will be per Airflow worker. Celery workers always keep minimum processes, but grow to maximum if necessary. Note the value should be max_concurrency,min_concurrency.You can have different settings for different worker node, based on resources on worker node and the nature of the task. If autoscale option is used, worker_concurrency will be ignored. 

### Airflow Task Pools [ [Ref](https://airflow.apache.org/docs/apache-airflow/stable/concepts.html#pools){:target="_blank"} ]
Airflow Pools can be defined using Airflow UI (Menu -> Admin -> Pools) or CLI. Tasks can then be associated with one of the existing pools by using the pool parameter when creating tasks in the DAG code:
     ```my_task = PythonOperator(pool='my_custom_pool')```

Airflow pools are typically used to limit the concurrency on specific types of task.  Some systems can get overwhelmed when too many processes hit them at the same time, e.g. downloading data from a webservice that limits concurrent connection. You can place limit by putting all such tasks to same pool and assigning a limit to the pool.
This is great if you have a lot of workers in parallel, but you don’t want to overwhelm a source or destination

The pool parameter can be used in conjunction with priority_weight to define priorities in the queue, and which tasks get executed first as slots open up in the pool. The default priority_weight is 1, and can be bumped to any number. When sorting the queue to evaluate which task should be executed next, we use the priority_weight, summed up with all of the priority_weight values from tasks downstream from this task. You can use this to bump a specific important task and the whole path to that task gets prioritized accordingly.

If tasks are not given a pool, they are assigned to a default pool default_pool which is initialized with 128 slots and can changed through the UI or CLI. 

