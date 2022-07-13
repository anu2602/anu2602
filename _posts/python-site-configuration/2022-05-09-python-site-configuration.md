---
title: Python 3 Site Configuration
tags: [Python, Simplified]
date: 2022-05-09 10:59:7 +05:30
description: Configuring Airflow Webserver UI to authenticate with Active Directory/LDAP. 
image: "/python-site-configuration/site-config.jpg"
comments: true

---
<figure>
<img src="site-config.jpg" alt="Site Configuration">
</figure>

This blog exists because the [official python site configuration documentation](https://docs.python.org/3/library/site.html){:target="_blank"} is not very clear. 

# site-packages
Python installation has a site-packages directory inside the module directory. This directory is where user installed packages are dropped. In other words when a new package is installed, the content of package are dropped into site-packages directory. It is the target directory of manually built Python packages. When you build and install Python packages from source (using distutils, probably by executing python setup.py install), you will find the installed modules in site-packages by default.

The `site` module handles site-specific configuration, especially the import path or module search path. `site` is automatically imported each time the interpreter starts up. On import, it extends `sys.path` with site-specific names constructed by combining the prefix values `sys.prefix` and `sys.exec_prefix` with several suffixes. The prefix values used are saved in the module-level variable PREFIXES for reference later. Under Windows, the suffixes are an empty string and `lib/site-packages`. For Unix-like platforms, the values are `lib/python$version/site-packages` (where $version is replaced by the major and minor version number of the interpreter, such as 3.5) and `lib/site-python`.

# Import Path
The search path for modules is managed as a Python list saved in sys.path. The default contents of the path include the directory of the script used to start the application and the current working directory.
```
import sys
for pdir in sys.path:
    print pdir
```


# PYTHONPATH
This is the most non-intrusive way of modifying the default search path. The format for setting the environment variable is same as the shell’s PATH: one or more directory pathnames separated by os.pathsep (e.g. colons on Unix or semicolons on Windows). Non-existent directories are silently ignored. In addition to normal directories, individual PYTHONPATH entries may refer to zipfiles containing pure Python modules (in either source or compiled form). Extension modules cannot be imported from zipfiles.

- on Unix like system : `echo $PYTHONPATH`
- on Windows : `echo %PYTHONPATH%`
- from within Python
```
import os
os.environ['PYTHONPATH']
```


# USER_SITE and PYTHONNOUSERSITE

## site.USER_SITE
Path to the user site-packages for the running Python, defaults are:
- `~/.local/lib/pythonX.Y/site-packages` for UNIX and non-framework macOS builds
- `~/Library/Python/X.Y/lib/python/site-packages` for macOS framework builds 
- ` %APPDATA%\Python\PythonXY\site-packages` on Windows. 
This directory is a site directory, which means that .pth files in it will be processed (discussed below).

## PYTHONNOUSERSITE 
The user site directory can be set through the PYTHONNOUSERSITE environment variable to override above platform-specific defaults.
site.USER_BASE and PYTHONUSERBASE are related variable to configure python base directory e.g `~/.local` on Unix. 

# site.addsitedir and sys.path.append
In the code following two mehtod could be used to modify the `sys.path`, or python import search path. 
1. sys.path.append('/path/to/search')
2. site.addsitedir('/path/to/site')

The difference between 1 and 2 is just plain appending is that when you use site.addsitedir, it also looks for .pth files within that directory. This directory can possibly be used to add additional directories to sys.path using .pth files (see below for more details). 


# .pth files
A path configuration file is a plain text file with the extension .pth. The paths in this file can be used to extend the import path to look for location that would not have been added automatically. It's contents are added to `sys.path`. Important points to note about .pth file.
- File must have extension of .pth e.g my-modules.pth
- File must be placed in site dir.
- Each line must contain a single path that will be appended to sys.path
- Non existant paths are ignored
- No check is made that the item refers to a directory rather than a file.
- Duplicates are ignored
- Blank lines and lines beginning with # are skipped
- Modules in the added directories will not override standard modules
- Paths can be absolute or relative
- Relative Paths are relative to the directory containing the .pth file
- Lines starting with import (followed by space or tab) are executed.
- An executable line in a .pth file is run at every Python startup.
- If a site directory contains multiple .pth files, they are processed in alphabetical order.


Example: 
```
site-packages
├── mypath.pth
└── my-site
    └── mymodule.py

```
mypath.pth file contains:
```
my-site
```


# sitecustomize.py and usercustomize.py

PTH files are only processed if they are in the site-packages directory ( to be precise in a "site directory"). Site directory itself is a  global setting for the Python installation and does not depend on the current/working directory. The site module is also responsible for loading site-wide customization defined by the local site owner in a sitecustomize module. Uses for sitecustomize include extending the import path and enabling coverage, profiling, or other development tools.

After these path manipulations, an attempt is made to import a module named sitecustomize, which can perform arbitrary site-specific customizations. sitecustomize.py is imported before Python starts running your own code. This can be used to add a new site dir:
```
import site
site.addsitedir('/path/to/site')
```
Similar to sitecustomize, the usercustomize module can be used to set up user-specific settings each time the interpreter starts up. usercustomize is loaded after sitecustomize, so site-wide customizations can be overridden.


## References
- [https://docs.python.org/3/library/site.html](https://docs.python.org/3/library/site.html){:target="_blank"}
- [site — Site-wide Configuration](https://pymotw.com/3/site/index.html){:target="_blank"}
- [Python Command line and environment](https://docs.python.org/3/using/cmdline.html){:target="_blank"}
