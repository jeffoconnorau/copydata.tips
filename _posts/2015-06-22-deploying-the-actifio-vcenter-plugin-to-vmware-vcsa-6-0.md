---
id: 162
title: 'Deploying the Actifio vCenter Plugin to VMware VCSA 6.0'
date: '2015-06-22T12:45:32+10:00'
author: joconnor
layout: post
guid: 'http://ruste.net/copydata.tips/?p=162'
permalink: /2015/06/deploying-the-actifio-vcenter-plugin-to-vmware-vcsa-6-0/
sbg_selected_sidebar:
    - 'a:1:{i:0;s:1:"0";}'
sbg_selected_sidebar_replacement:
    - 'a:1:{i:0;s:7:"Sidebar";}'
dsq_thread_id:
    - '3868117311'
tags:
    - Actifio
    - Plugin
    - vCenter
    - VCSA
---

This is just a short tip to help get the Actifio vCenter Plugin uploaded to your VCSA appliance, so you can start the installation process.

By default if you try to scp the install file you will find an error such as the following:

```
<tt>Unknown command: 'scp'</tt>
```

1\. Login via SSH to the vCenter Server Appliance on port 22, I would normally use the root account here:

```
<tt>$ ssh root@10.0.0.10

VMware vCenter Server Appliance 6.0.0

Type: vCenter Server with an embedded Platform Services Controller

Password:
Last login: Mon Jun 22 00:15:14 UTC 2015 from 10.0.0.51 on ssh
Last failed login: Mon Jun 22 02:38:16 UTC 2015 from 10.0.0.51 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Mon Jun 22 02:38:17 2015 from 10.0.0.51
Connected to service

    * List APIs: "help api list"
    * List Plugins: "help pi list"
    * <strong>Enable BASH access: "shell.set --enabled True"</strong>
    * <strong>Launch BASH: "shell"</strong>

Command></tt>
```

2\. As per the motd, you need to start the shell so run the command

```
<tt>shell.set --enable True</tt>
```

Then run the following command to get into the shell

<tt>shell</tt>

Next, run the following command to change the default shell

<tt>chsh -s "/bin/bash" root</tt>

Now you can scp the Actifio VCP file to the vCenter Server Appliance. FileZilla or command line scp are your friend here (example below):

<tt>scp -P 22 ActifioVCPInstaller\_unix\_6\_1\_2.sh root@10.0.0.10:/tmp/.  
</tt>

Now go back to the VCSA shell and change the default shell back

<tt>chsh -s /bin/appliancesh root</tt>

Now you can run the installer with the following command

<tt>sh /tmp/<tt>ActifioVCPInstaller\_unix\_6\_1\_2.sh</tt></tt>

And now follow the bouncing ball to get your Actifio vCenter Plugin installed.