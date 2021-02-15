---
title: Airflow AD/LDAP Integration.
tags: [Airflow, Prod]
date: 2020-04-05 11:58:47 +07:00
description: Configuring Airflow Webserver UI to authenticate with Active Directory/LDAP. 
image: "/airflow-concurrency-simplified/airflow.jpeg"
comments: true

---

If you are in enterprise environment, chances are you are already using Active Directory as standard authentication. Itegrating Airflow with your Enterprise Active Directory/SSO/LDAP will not only simplify user managementin Airflow but is also very convenient for users. They no longer need to remember yet another username/password. 

<figure>
<img src="door.jpg" alt="confused">
<figcaption style="color: grey !important;"> 
	Photo by <a href="https://unsplash.com/@pechka" style="color: grey !important;" target="_blank">Dima Pechurin
</a> 
</figcaption>
</figure>

### Pre-requisite 
It is assumed that you have a working airflow setup with
```airflow version >= 1.10.2 ``` and ```python version >= 3.6 ```

You will also need `python-ldap`, which can be installed using `pip install python-ldap` or `conda install python-ldap`

`python-ldap` package is based on OpenLDAP, so you need to have the development files (headers) in order to compile the Python module. 

*Debian/Ubuntu:*<br/>
```sudo apt-get install libsasl2-dev python-dev libldap2-dev libssl-dev```

RedHat/CentOS:<br/>
```sudo yum install python-devel openldap-devel```


Airflow 1.10+ uses Flask-AppBuilder (FAB) for user interface. It is expected and obvious that the configuration follows FAB configuration. So if you run into issues, it would be worth  [ Searching Flask AppBuilder LDAP](https://www.google.com/search?q=Flask-AppBuilder%20LDAP){:target="_blank"} instead of Airflow LDAP.



Make sure you have `rbac = true` in `airflow.cfg`.

Sdd `webserver_config.py` in `AIRFLOW_HOME`, with following contents.
```
import os
from airflow import configuration as conf
from flask_appbuilder.security.manager import AUTH_LDAP

# Do not delete this line, reqired by FAB for creating users in DB.
SQLALCHEMY_DATABASE_URI = conf.get('core', 'SQL_ALCHEMY_CONN')

CSRF_ENABLED = True

AUTH_TYPE = AUTH_LDAP

AUTH_ROLE_ADMIN = 'Admin'

#Allows user to register on first login
AUTH_USER_REGISTRATION = True

# User will have, viewer role on first login
# Options are Admin, Viewer, User, Op
# If you have custom role, you can use that too.
AUTH_USER_REGISTRATION_ROLE = "Viewer"

# Replace with your LDAP Server
AUTH_LDAP_SERVER = 'ldaps://myldapserver:636/' 

# Replace with your LDAP Base DN
AUTH_LDAP_SEARCH = 'DC=domain,DC=org,DC=com'

# Replace with your LDAP entry for service user / LDAP Bind User
AUTH_LDAP_BIND_USER = 'CN=ServiceUser,OU=serviceAccount,DC=domain,DC=org,DC=com'
# Bind user Password
AUTH_LDAP_BIND_PASSWORD = '**************'

# LDAP field used for UID
AUTH_LDAP_UID_FIELD = 'CN'

#TLS Settings, configure as per your LDAP/AD setup
AUTH_LDAP_USE_TLS = False
AUTH_LDAP_ALLOW_SELF_SIGNED = False
AUTH_LDAP_TLS_CACERTFILE = '/path/to/root_CA.crt' #You may not need this
```

### References
1. [Airflow pull-request for AUTH_LDAP implementation](https://github.com/dpgaspar/Flask-AppBuilder/pull/1374){:target="_blank"}
2. [Flask-AppBuilder supported authentication types](https://flask-appbuilder.readthedocs.io/en/latest/security.html){:target="_blank"}
3. [python-ldap reference documentation](https://www.python-ldap.org/en/latest/reference/index.html){:target="_blank"}
4. [Flask-AppBuilder configuration reference](https://flask-appbuilder.readthedocs.io/en/latest/config.html){:target="_blank"}
