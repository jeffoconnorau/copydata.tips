---
id: 34
title: 'Windows System State Backups with Actifio'
date: '2014-04-15T05:37:28+10:00'
author: joconnor
layout: post
guid: 'http://ruste.net/copydata.tips/?p=34'
permalink: /2014/04/windows-system-state-backups-actifio/
dsq_thread_id:
    - '2689403036'
tags:
    - Actifio
    - 'System State'
---

Say Whaaaaa ?

Yes you heard correctly. Due to numerous customer requests I researched this capability, while it’s not efficient entirely if you include it as part of a daily backup, but we can do it. Read on if you want more.

I said it’s not efficient, as the process is essentially creating a new compressed file everyday (10GB+ is not uncommon). What this will do is cause a lot of new unique data to enter into the dedup pool. But here is the thing, we have a few ways to provide the capability without the storage consumption penalty.

I will continue to research further to see if we can reduce the file size somehow with Microsoft, but here is what’s possible today.

This procedure should work for Windows Server 2008+ and is fairly simple.

1\. Local command line backup facilities must be enabled. If they are not these can be added via the GUI, or else run in an administrator command prompt the following command:

```
C:\Windows\>servermanagercmd -install Backup-Tools
```

2\. Run a pre script via VMware Tools, or the Actifio Connector script, with the command in it.

```
wbadmin start -systemstate -backuptarget:D:\ -quiet
```

In the above example the D:\\ must exist as a target, but this could easily be changed for the Actifio Staging disk (or mount point) as the target.

In future, I hope to further refine the testing to only include the last 1-3 days of data in the local system for recovery and 2, exclude the backup of all files in the C:\\Windows\\ folder and items beneath it.

I would strongly suggest customers don’t do this on every server, and run it daily, but it might form as an additional policy to be run weekly on a domain controller (PDCe) as an example if required on a regular basis.

Actifio also supports Bare Metal Recovery, which includes System State Backups as part of the licensing. So this article itself is not part of that solution, as it is built in.

Now another approach to solving the idea, without storage penalty you could do the following and it applies only for Virtual Machines.

1\. Ensure the machine that you want a system state backup for, has an SLA policy applied that includes the snapshot option. While this is not strictly mandatory, it will help with the speed of recovery for any system state image, as the machine can be mounted back instantly.

2\. When you need a system state item, you can do an instant mount of the VM from the point in time you need to recover from. Mount the VM back to a hypervisor and as usual the VM will not be attached to the network.

3\. Power on the VM and connect to the console session. Then login using local admin or cached credentials.

4\. Run the command as above:

```
wbadmin start -systemstate -backuptarget:D:\ -quiet
```

This will create a system state backup from the VM at the time the snapshot backup was initiated.

5\. Then Power down the Virtual Machine.

6\. Attach the D:\\ VM Disk (or disk where you stored the system state backup image) to any other running VM, normally best to attach to the same VM as where the backup was done, and attach the drive to the running VM.

7\. You can then proceed with the System State restore process that you would normally follow, as documented by a Microsoft KB article or TechNet article.

8\. Once completed you can Unmount and delete the volume from the running VM, and as a best practise it’s probably wise to initiate a manual backup of the VM/server where the recovery was performed.

The benefit to the VM Instant mount approach, is that you still have access to the data, even though it’s not explicitly stored in the dedup pool as a new unique blocks every day. This will save storage in your long term retention pool, and only add an additional few steps in the recovery of system state restores. The instant mount feature Actifio provides greatly reduces the time to recover system state data, along with any files, databases, VM Disks, or entire VMs in seconds.