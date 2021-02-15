---
title: Jupyter Hub AD/LDAP Integration.
tags: [Jupyter Hub, Prod]
date: 2020-04-14 11:58:47 +07:00
description: Configuring Jupyter Hub to authenticate with Active Directory/LDAP. 
image: "easy.jpg"
comments: true

---

If you are in Enterprise environment, chances are you are already using Active Directory as standard authentication. Itegrating Jupyter Hub with you Enterprise Active Directory/SSO/LDAP will not only simplify user management in Jupyter Hub but is also very convenient for users. They no longer need to remember yet another username/password. 

Also it is worth mentioning upfront that it is very easy and [documentation](https://github.com/jupyterhub/ldapauthenticator){:target="_blank"} is pretty good. 

<figure>
<img src="easy.jpg" alt="That's Easy">
<figcaption style="color: grey !important;"> 
	Photo by <a href="https://unsplash.com/@oskarmalm" style="color: grey !important;" target="_blank">Oskar Malm
</a> 
</figcaption>
</figure>

### Pre-requisite 
It is assumed that you have a working jupyter-hub setup with
```jupyterhub version >= 1.1.0``` and ```python version >= 3.6 ```


### Set-up
First of all install jupyterhub-ldapauthenticator using: <br/>
```pip install jupyterhub-ldapauthenticator```
<br/>or <br/>
```conda install -c conda-forge jupyterhub-ldapauthenticator```


You will also need `ldap3` module, which can be installed using `pip install ldap3` or `conda install ldap3`. It is nice to see jupyterhub folks choosing to use ldap3, which is pure python ldap library. 


Add following to `jupyterhub_config.py` in `JUPYTERHUB_HOME`.
```
	c.LDAPAuthenticator.lookup_dn = True
	c.LDAPAuthenticator.lookup_dn_search_filter = '({login_attr}={login})'
	c.LDAPAuthenticator.lookup_dn_search_user = 'ldap_search_user_technical_account'
	c.LDAPAuthenticator.lookup_dn_search_password = 'secret'
	c.LDAPAuthenticator.user_search_base = 'ou=people,dc=achowdhary,dc=com'
	c.LDAPAuthenticator.user_attribute = 'sAMAccountName'
	c.LDAPAuthenticator.lookup_dn_user_dn_attribute = 'cn'
	c.LDAPAuthenticator.escape_userdn = False
	c.LDAPAuthenticator.bind_dn_template = '{username}'

	c.Authenticator.admin_users = {'anuradha', 'admin'}
```


