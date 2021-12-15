---
title: Create a Virtual Mount on Linux. 
date: 2020-01-05 11:58:47 +05:30
tags: [Linux, HowTo]
description: A short step by step process for creating a virtual mount or direcotry with size limit.
comments: true
---

Sometime you want to create a directory with upper limit on size in linux, this could be to restrict usage of a applicaiton or a user or for any other reason. There is no direct way to set limit on quota/size limit on a direcotry in linux. But as the saying goes, you can do anything on linux. 

One common way of having size limit directories is creating a virual mount point. Here are the steps to create Vitual Mount Point. Here are the steps for the same:

 - Create a directory for filesystem image `mkdir /vdisk`
 - Create a filesystem on disk, my preferred way is to use `fallocate` <br>
`fallocate -l 100G /vdisk/my_disk.img`<br>
Alternatively you can use `dd` <br>
`dd if=/dev/zero of=/vdisk/my_disk.img`
 - Create fileystem on the disk image, I use `xfs` but you can use ex3 or ext4. <br>
`mkfs.xfs /vdisk/my_disk.img` 
 - Create the desired directory to mount the disk. <br>
`mkdir -p /opt/data/my_disk` 
 - Mount the virtual disk. <br> `mount -t auto -o loop /vdisk/my_disk.img /opt/data/my_disk` <br>
or <br/> `mount -t auto -o loop,rw,usrquota,grpquota /vdisk/my_disk.img /opt/data/my_disk` <br>
If you would like to set user or group quotas.
 - Add the mount to `/etc/fstab` to make it permanent. 
