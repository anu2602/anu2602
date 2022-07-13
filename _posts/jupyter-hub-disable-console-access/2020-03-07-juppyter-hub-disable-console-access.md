---
title: Disable Console Access for Jupyter Hub. 
date: 2021-03-07 09:58:47 +07:00
tags: [Jupyter Hub, Jupyter Lab]
description: Disable Terminal / Console / Shell access from Jupyter Hub.  
image: "/jupyter-hub-disable-console-access/no-access.jpg"
comments: true

---
<figure>
<img src="no-access.jpg" alt="You are not allowed!"> 
<figcaption style="color: grey !important;"> 
	Photo by <a href="https://unsplash.com/@fionakiwi" style="color: grey !important;" target="_blank">Fiona Jackson
</a> 
</figcaption>
</figure>

#### Can you disable terminal  access in Jupyterhub?
The answer is not a simple Yes or No. It's is a bit compliated and hence this post. 

#### Can you stop showing Terminal from Jupyter UI?

This has a simple answer ☺️ :relaxed:. However ther is no configuration, the configuration `NotebookApp.terminals_enabled=False` works in single user notebook but does not work in `jupyterhub` \ `jupyterlab` setup. Fortunately there is a simple way to achieve this. If you uninstall a package called 'terminado', terminals will disappear.

```
	pip uninstall -y terminado
```

#### Does this secure my Jupyter Hub Setup?

No, because anything you can do from the terminal, you can do from a notebook anyway, so don't rely on this as any kind of security measure. A user can: 
- open a Python notebook and use the %%bash or ! shell magics.
- open a Python notebook, import subprocess, and execute arbitrary commands.
- open a notebook for pretty much any other language and execute a shell child process (e.g., s"ls -l" ! in Scala).

Even if you could turn off magic shell commands Python itself has the permissions to do the exact same things,so there's very little point in even trying. There is no practical way to block the user from running system commands. Even JupyterHub, is designed with the use-case of semi-trusted users, and requires very careful set up to allow for untrusted users. From Jupyter Hub security Documentation: 

> JupyterHub is designed to be a simple multi-user server for modestly sized groups of semi-trusted users. While the design reflects serving semi-trusted users, JupyterHub is not necessarily unsuitable for serving untrusted users.


#### What is my best options for securing my JupyterHub?
If you are letting untrusted users execute code on your system, the safest approach is to run inside container without access to the system outside the container. JupyterHub has support for containerisation of notebook servers, so each user will have their own (e.g. Docker) container and you can limit access to files. If they mess up their own container it would not mess up other users or the main server. It would  alsow be relatively easy to setup backup and restore data for users. 

Since much of the point of IPython and Jupyter is arbitrary code execution, the security model for deploying Jupyter ought to be applying restrictions at the process/container level, rather than disabling  UI for running commands.

But setting up JupyterHub instance with docker is not trivial and if you are dealing with very small team of trusted users, you should try to train and guide users. They would most likely anyway have access to server with sudo access. 


#### Is there any other alternative?
If you want to use Linux Containers for isolation and security benefits, but don't want the headache and complexity of container image management, then you could use the `SystemdSpawner`. However please note that the Jupyterhub must be run as `root` to use `SystemdSpawner`.

You could also use `SudoSpawner` for isolation but `SystemdSpawner` provides more features, options and isolation support including memory, CPU and `/tmp` isolation. 

#### Is there any simple hack?

There is a hack to prevent users from launching a shell by setting setting the SHELL of the user or process to `/bin/false`; This will prevent any shell from launching a shell. This can work well with `SystemdSpawner` by setting `SHELL=/usr/false` in the systemd service file. But please remember this is a hack and resourceful developer will be able to find a workaround. 

### References
1. [Jupyter Hub Security Overview](https://jupyterhub.readthedocs.io/en/stable/reference/websecurity.html){:target="_blank"}
2. [Jupyter Lab Documentation](https://jupyterlab.readthedocs.io/en/stable/){:target="_blank"}
3. [Jupyter Hub Docker Spawner](https://jupyterhub-dockerspawner.readthedocs.io/en/latest/){:target="_blank"}
4. [Zero to Jupyter Hub](https://github.com/jupyterhub/zero-to-jupyterhub-k8s){:target="_blank"}
5. []Jupyter Hub Systemd Spawner](https://github.com/jupyterhub/systemdspawner){:target="_blank"}



