---
title: Jupyter Hub setup with Jupyter Lab. 
date: 2020-07-07 11:58:47 +07:00
tags: [Jupyter Hub, Jupyter Lab]
description:  Setting up a working Jupyter Hub with Jupyter Lab on RedHat Enterprise Linux 7 (RHEL 7).  
image: "hub-lab.jpg"
comments: true
---
Jupyter Hub is the best way to serve Jupyter Notebook to multiple users and Jupyter Lab is the next generation web interface for project Jupyter. I have set it up for multiple groups, data engineers, students, kids learning python and they all love it because of simplicity and ability to easily share python code. 

<figure>
<img src="hub-lab.jpg" alt="JupyterLab and JupyterHub"> 
</figure>

#### Pre-requisite 
You have working python setup with ```python version >= 3.6 ```


#### Install configurable-http-proxy
```
	sudo yum install rh-nodejs10
	sudo npm install -g configurable-http-proxy
```

#### Install Jupyter Hub and Jupyter Lab
```
	pip install jupyterhub jupyterlab
```
or 
```
	conda install jupyterhub jupyterlab
```

#### Configuration and setup
Create a directory for you jupyter hub. Let's assume the dir is `/opt/jupyter`
```
	mkdir /opt/jupyter
	cd /opt/jupyter
	export JUPYTERLAB_DIR = /opt/jupyter
```
I would recommed adding above variable to your `.bashrc` (if you are using bash).
	`echo "export JUPYTERLAB_DIR = /opt/jupyter" >> ~/.bashrc`

`jupyterhub --generate-config` <br/>
`jupyter lab --generate-config` <br/>

This will generate `jupyterhub_config.py` and `jupyter_notebook_config.py`. I highly recommend deleteing all commented lines from the generated config file. Keeping only required lines, makes it much easier to maintain it. To delete all commented lines
```
	sed -i '/^#/ d' jupyterhub_config.py
	sed -i '/^\s*$/d' jupyterhub_config.py
	sed -i '/^#/ d' jupyter_notebook_config.py
	sed -i '/^\s*$/d' jupyter_notebook_config.py
```

Edit `jupyterhub_config.py` in your favorite editor and make following changes.
```
	c.Spawner.default_url = '/lab'
	c.JupyterHub.hub_ip = '0.0.0.0'
	c.JupyterHub.spawner_class = 'jupyterhub.spawner.SimpleLocalProcessSpawner'
```
Edit `jupyter_notebook_config.py` in your favorite editor and make following changes.
```
	c.NotebookApp.allow_remote_access = True
	c.NotebookApp.ip = '0.0.0.0'
```
#### Adding Jupyter Kernels
If you have existing jupyter kernels that you would like to use with your setup you need to copy them to `$PYTHON_HOME/share/jupyter/kernels/`. You can also optionally place kernal logos inside your kernal folders to make your jupyter setup look colorful. The logo files should be named  `logo-32x32.png` and `logo.64x64.png`

<figure>
<img src="lab-logos.png" alt="Kernel Logos in Jupyter Lab"> 
</figure>


#### Configuring SSL/https
Generate a Private key and cert using your preferred method. e.g.<br/>
`openssl req -x509 -out my.crt -keyout my.key -newkey rsa:2048 -nodes -sha256`

Add following lines to `jupyterhub_config.py` <br/>
```	
	c.JupyterHub.ssl_key = '/path/to/my.key'
	c.JupyterHub.ssl_cert = '/path/to/my.cert'
```
#### Starting Jupyter Hub
You need to ensure that the nodejs that you installed in enabled. You can do this by doing 
`scl enable node rh-nodejs10 bash`, but I prefer source `source /opt/rh/rh-nodejs10/enable` as it does not open a new shell inside your current one. You can also consider adding this to your `.bashrc` (if you are using bash).
	`echo "source /opt/rh/rh-nodejs10/enable" >> ~/.bashrc`

to start Jupyter Hub:
```
	cd $JUPYTERLAB_DIR
	nohup jpyterhub &
```
#### URLs:
Jupyter Notebook Url: `http(s)://<your_server>:8000` <br/>
JupyterHub Admin Url: `http(s)://<your_server>:8000/hub/admin`

#### Stopping Jupyter Hub
 - Open Hub admin in browser `http(s)://<your_server>:8000/hub/admin` 
 - Click **Shutdown Hub** button
 - Check both *Shutdown Proxy* and *Shutdown single-user-servers*
 - Click **Shutdown**

<!--
TODO add disable Terminal
jupyter labextension list
jupyter labextension uninstall terminal
jupyter labextension disable terminal
jupyter lab build
vi setttings/page_config.json
{
	"terminalAvailable": false,
	"disableExtensions" : [
	 ```
	 yum install rh-nodejs10
	 ```"@jupyterlab/terminal",
	 "terminal"
	]
}

jupyter/lab/schemas/@jupyterlab/terminal-extension
plugin.json
consoloe-extension
setttings/package_config.json
-->


### References
1. [Jupyter Hub Documentation](https://jupyterhub.readthedocs.io/en/stable/){:target="_blank"}
2. [Jupyter Lab Documentation](https://jupyterlab.readthedocs.io/en/stable/){:target="_blank"}
3. [Jupyter Hub Security Configuration](https://jupyterhub.readthedocs.io/en/stable/getting-started/security-basics.html){:target="_blank"}




