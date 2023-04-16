---
id: 41
title: 'vCenter PowerShell PowerCLI command to to change a VMDK disk from non-persistent to persistent without powering down the VM'
date: '2014-04-17T11:12:24+10:00'
author: joconnor
layout: post
guid: 'http://ruste.net/copydata.tips/?p=41'
permalink: /2014/04/vcenter-powershell-powercli-command-change-vmdk-disk-non-persistent-persistent-without-powering-vm/
dsq_thread_id:
    - '2617702974'
tags:
    - PowerCLI
    - Powershell
---

While I am certain this already posted on the net in a few places, I wanted to capture it here, as it is a question I get asked often by customers. Especially seeing as the manual method requires shutting down a VM to make the change.

<span style="text-decoration: underline;">Instructions</span>

Just replace the text <span class="s1" style="font-weight: inherit; font-style: inherit;">*&lt;vcenter\_dns\_or\_ipaddress&gt;* </span>with the IP address or FQDN of the vCenter host. You will be prompted to login to vCenter, and will require enough privileges to modify the VM you need to edit.

```
<pre class="p3" style="color: #3d3d3d;"><span style="font-weight: inherit; font-style: inherit;">connect-viserver <em><vcenter_dns_or_ipaddress></em>
</span>
```

Then replace the <span class="s1" style="font-weight: inherit; font-style: inherit;">*&lt;vm\_name&gt;* </span>with the name of the virtual machine you wish to change one or many of its disks to Independent Persistent. It will provide multiple prompts based on how many disks the VM has, and ask you to confirm each one individually, or all if desired.

```
<pre class="p3" style="color: #3d3d3d;"><span style="font-weight: inherit; font-style: inherit;">Get-HardDisk -VM <em style="font-weight: inherit;"><vm_name></em> | Set-HardDisk -Persistence "IndependentPersistent"</span>
```

Then if required, run the command below to change the disk back to Persistent, and again select Y for each disk you want set back.

```
<pre class="p3" style="color: #3d3d3d;"><span style="font-weight: inherit; font-style: inherit;">Get-HardDisk -VM <em style="font-weight: inherit;"><vm_name></em> | Set-HardDisk -Persistence "Persistent"</span>
```