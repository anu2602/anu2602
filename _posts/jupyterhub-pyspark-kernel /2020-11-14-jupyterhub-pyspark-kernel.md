---
title: Pyspark Kernel with Jupyter
date: 2020-11-14 13:03:33 +05:30
tags: [jupyter, HowTo, spark]
description: Steps to setup Pyspark Kernel with Jupyter.
image: "/jupyterhub-pyspark-kernel/sparks.jpg"
comments: true

---
<figure>
<img src="spacex.jpg" alt="Vi Cheat Sheet">
<figcaption style="color: grey !important;"> 
        Photo by <a href="https://unsplash.com/@spacex" style="color: grey !important;" target="_blank">SpaceX</a> 
    </figcaption>
</figure>

## Install Java and Scala
```
        scala -version
        java -version
```

## Install Java and export
```
        export JAVA_HOME=/usr/lib/jvm/java-8-oracle
        export JRE_HOME=/usr/lib/jvm/java-8-oracle/jre
```


## Install py4j
```
        pip install py4j
```

## Install py4j
```
        pip install py4j
```

## Create pyspark kernel JSON
```
        {
         "display_name": "PySpark",
         "language": "python",
         "argv": [
          "<python_path>",
          "-m",
          "ipykernel",
          "-f",
          "{connection_file}"
         ],
         "env": {
          "SPARK_HOME": "<spark_home>",
          "PYTHONPATH": "<spark_home>/python/:<spark_home>/python/lib/py4j-0.9-src.zip",
          "PYTHONSTARTUP": "<spark_home>/python/pyspark/shell.py",
          "PYSPARK_SUBMIT_ARGS": "--master local[*] pyspark-shell"
         }
        }
```
## References
- <https://jupyter-client.readthedocs.io/en/stable/kernels.html>
- <https://pypi.org/project/pyspark-kernel/>