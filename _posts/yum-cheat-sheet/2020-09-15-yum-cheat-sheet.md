---
title: Yum Cheat Sheet
date: 2020-09-15 21:22:42 +05:30
tags: [yum, HowTo, CheatSheet]
description: A short collection of common yum commands and tips.
image: "/yum-cheat-sheet/yum-cheat-sheet.jpg"
comments: true

---
<figure>
<img src="yum-cheat-sheet.jpg" alt="Git Cheat Sheet">
</figure>
# Yum Cheat Sheet

Yum (Yellow Dog Updater, Modified), is the package manager used on RPM (RedHat Package Manager) based Linux systems such as CentOS, RHEL. It is an interactive, rpm based, package manager. It can automatically perform system updates, including dependency analysis and obsolete processing based on "repository" metadata. 

#### Getting Help

Use `yum help [command]' to get help on yum commands and options, you can also use `man yum'.

```
yum help
yum help install
yum help erase
```

## Updating Packages

- Check the whole system for available updates:
```
sudo yum check-update
```

- Update the whole packages to the latest version:
```
sudo yum update
```

- Apply only security related updates:
```
sudo yum update --security
```

- Update an individual package:
```
sudo yum update [package_name]
```

- Update all packages in a group:
```
sudo yum groupupdate [group_name]
```

- Update an individual package to an specific version:
```
yum --showduplicates list [package_name]
sudo yum update-to [package-version]
```

- Update a package from a specific repository: 
```
sudo yum repo-pkgs [repo] upgrade [package_name]
```

#### Managing Repositories
- Add a `[repository]` section to the `/etc/yum.conf` file, or to a `.repo` file in the `/etc/yum.repos.d/` directory.
- Remove a `[repository]` section to the `/etc/yum.conf` file, or to a `.repo` file in the `/etc/yum.repos.d/` directory.
- Display enabled repositories: `yum repolist`
- Display all enabled/disabled repositories: `yum repolist all`
- Download yum repository data to cache: `yum makecache`
- Get information about repositories:
```
$ yum repoinfo
$ yum repoinfo <repo-id>
```
- List all packages in the specified repository:
```
 yum repo-pkgs <repo id> list
```

#### Viewing Package Info

- List all installed packages:
```
yum list installed [package_name]
yum list installed 
```
- List all available packages:
```
yum list available
```
- List installed and available kernel packages
```
yum list kernel
```
- List dependencies of a package:
``` 
yum deplist [package_name]
```
- Search packages by name:
```
yum search [string]
yum search [package_name]
```

- Find packages that provide the queried file:
```
yum provides <string>
yum provides <foldername/filename>
yum provides <filename>
```

- Display information about a package: `yum info [package_name]`


#### Install & Remove Packages

- Install a package from repository:
```
sudo yum install [package_name]
sudo yum install [p1] [p2]
sudo yum --enablerepo=[repo] install [package_name]
sudo yum repo-pkgs [repo] install [package_name]
```
- Install a local rpm package:
```
sudo yum localinstall [package_name].rpm
sudo yum localinstall [http://myrepo.org/packagename.rpm]
```
- Remove an installed package:
```
sudo yum remove [package_name]
sudo yum remove [p1] [p2]
```


- Remove one package and install another:
```
sudo yum swap [package_to_rmove] [package_to_install]
```
- Remove unwanted packages and dependencies: `sudo yum autoremove` 
- Remove all packages installed from a specific repository: 
```
sudo yum repo-pkgs [repo] remove
```

#### History

- View all the past transactions of yum command:
```
sudo yum history
sudo yum history list
```
- Show details of yum transaction ID:
```
sudo yum history info <ID>
```
- Undo the yum action from transaction ID:
```
sudo yum history undo <ID>
```
- Redo the undone yum action from transaction ID:
```
sudo yum history redo <ID>
```

#### Clean
- Clear out cached package data:
`sudo yum clean all`
- Delete packages saved in cache:
`sudo yum clean packages`

#### General Options

 * `-y` Assume that the answer to any question  which  would be asked is yes.
 * `--assumeno` Assume that the answer to any question which would be asked is no.
 * `-q` Run without output.  Note that you likely also want to use -y.
 * `-v` Run with a lot of debugging output.
 * `--showduplicates` Doesn’t limit packages to their latest versions in the info, list and search commands
 * `--enablerepo` Enable currently disabled repo for a single command
 * `--disablerepo` Disable currently enabled repo for a single command
 * `--downloadonly` Don’t update, just download to `/var/cache/yum/arch/prod/repo/packages/`
 * `--changelog` Display changelog information of package

#### Related Files
```
/etc/yum.conf
/etc/yum/version-groups.conf
/etc/yum.repos.d/
/etc/yum/pluginconf.d/
/var/cache/yum/
```

