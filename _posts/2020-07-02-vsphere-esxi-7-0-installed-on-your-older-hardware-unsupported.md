---
id: 214
title: 'vSphere / ESXi 7.0 installed on your older hardware (unsupported)'
date: '2020-07-02T02:13:45+10:00'
author: joconnor
layout: post
guid: 'https://copydata.tips/?p=214'
permalink: /2020/07/vsphere-esxi-7-0-installed-on-your-older-hardware-unsupported/
sbg_selected_sidebar:
    - null
sbg_selected_sidebar_replacement:
    - null
tags:
    - esxi
    - vexpert
    - VMware
---

Let me preface this post by saying, this is not a supported way to get vSphere / ESXi 7.0+ running on legacy hardware. Please only perform these actions if you are comfortable with the steps below, and know that you will not be entitled to support for VMware.

For my example in this article, I am using an IBM M3 X3550 Series server. While this approach might also work for other vendor hardware, you may need to make some slight modifications to the process, but it should still work.

**<u>TL;DR</u>** – You basically need to do two things, 1. Install ESXi onto a USB drive, instead of local hard drives, and 2. Implement the `allowLegacyCPU=true` mode in the boot.cfg.

Here are the list of things you need:

1\. The VMware vSphere ISO to use for the installation. I used this one I downloaded from VMware “VMware-VMvisor-Installer-7.0.0-15843807.x86\_64.iso”  
2\. A 16Gb or larger USB flash drive. I would recommend you use something decent here. In my example. I was using a TDK Silver key I had spare at home, but this was a fairly reliable key, and also not used often.  
3\. Common ssh and shell command familiarity, editing files with vi, General understanding of VMware ESXi installation processes, etc.

Sorry for the photos instead of screenshots. I will try and update these with better screenshots in future.

First thing is first, I went into the IMM bios for this server and changed the boot order as per the photo.  
CD/DVD first, then USB second, and Hard Disks later. While my server has 2x 15K 300GB disks in a Raid 1 mirror, these can’t be used as the VMware Hypervisor install location, as the Raid card is no longer supported in ESXi 7.0 HCL.

![](https://copydata.tips/wp-content/uploads/2020/07/20200701_203049-1024x692.jpg)

After this is completed, you can then commence the installation, as there is no Physical DVD/CDRom on the server, I am mounting the ISO via my Windows 10 Virtual Machine Desktop. Historically for these legacy IBM servers, the best solution is to use Windows and use Internet Explorer as the browser for connecting to the IMM IP address, make sure you add the IP address to the Trusted Sites list in IE, and run a version of Java like 1.7.0\_60. I also like to add the IP address for the IMM to the Java Exception Site List.

It is a good time to insert your USB key into the server now. It will be formatted by the ESXi installation, so no need to prep it ahead of time. Just make sure it doesn’t have any important files first.

OK, but if you have made is this far you should be ready to boot the machine, and boot from the ISO or Physical CD of VMware Hypervisor v7.0.x, but you will need to stop the booting of the ESXi Installer via the “SHIFT and O” thats o for oh, not a zero. You should see the prompt for this, and you need to run it, else the installer will halt and error out anyway, so if you missed the “SHIFT + O” prompt, reboot the server.

Once you get to the prompt, you will need to manually add the text `allowLegacyCPU=true` to the command, after the runweasel text as below. It’s also very important to type it exactly, as this is case-sensitive. Many thanks to William Lam for this key ingredient. He shared this post [here:](https://www.virtuallyghetto.com/2020/04/quick-tip-allow-unsupported-cpus-when-upgrading-to-esxi-7-0.html) and it is well worth a read.

![](https://copydata.tips/wp-content/uploads/2020/07/20200701_221438-1024x821.jpg)

Now the installation should commence…

![install proceeding](https://copydata.tips/wp-content/uploads/2020/07/20200701_221629-1024x811.jpg)

When prompted as to the location for the installation, you should see the option to install to your USB flash drive. If you don’t see it under Local: then you may need to try a different USB flash drive.

![usb format](https://copydata.tips/wp-content/uploads/2020/07/20200701_221808-1024x815.jpg)

You will likely be warned about incompatible hardware in the server. Hit Enter to continue, and please note again, this work around should never be used for critical systems!

![warnings](https://copydata.tips/wp-content/uploads/2020/07/20200701_221947-1024x793.jpg)

Now we are prompted to confirm the installation to the USB flash drive, and the partitions will be lost. Hit F11!

![confirming install](https://copydata.tips/wp-content/uploads/2020/07/20200701_222007-1024x786.jpg)

Now, you can complete the installation and set a management IP address, and any other settings as you normally would.

![completed install](https://copydata.tips/wp-content/uploads/2020/07/20200701_222606-1024x818.jpg)

On the next boot up after the install, you will likely need to insert the “allowLegacyCPU=true” option one more time upon boot, using the “SHIFT + O” keys to modify the prompt.

![shift-o](https://copydata.tips/wp-content/uploads/2020/07/20200701_222935-1024x821.jpg)

Now ESXi should boot up and you all good, but wait theres one more step to ensuring you don’t need to modify the boot command prompt every time you restart the host. Read on !

So we need to edit the boot.cfg file to achieve this. So you need to start the TSM and TSM-SSH services to be able to connect via ssh to your ESXi server. A default install, will have these services set to stopped. So login to your ESXi host via the Management IP address you set and select the Manage Host option on the left menu, then Services tab, and start the services. Here is the before.

![stopped services](https://copydata.tips/wp-content/uploads/2020/07/esxi-services-1024x267.jpg)

Here is the after.

![services started](https://copydata.tips/wp-content/uploads/2020/07/esxi-services-on-1024x259.jpg)

Now you can ssh in as root, using the password you set during the installation.

In my following screenshots I looked for these files, and then edited them manually. The best way is to run a shell command `find / -name "boot.cfg"` and then modify both boot.cfg files with appending the allowLegacyCPU=true to the kernelopt= line. See my screenshots below. The first one, I found one file and edited it, then checked it with `cat boot.cfg` to make sure it was good.

![boot edit](https://copydata.tips/wp-content/uploads/2020/07/20200701_224228-1024x755.jpg)

Then I used the find command to see where the other boot.cfg file was. Now these files, should actually just be in the /bootbank and /altbootbank symlinked directories. I wanted to be sure, so used find instead. It might not be also required to edit both bootbanks.

![second edit](https://copydata.tips/wp-content/uploads/2020/07/20200701_224404-1024x679.jpg)

Lastly, now check both files are updated, and give the ESXi server a reboot from the ESXi UI, and test that the server comes back up after a reboot without any user input.

![last check](https://copydata.tips/wp-content/uploads/2020/07/20200701_224423-1024x795.jpg)

And here is the money shot. ESXi 7.0 running on a Legacy server, ready for some testing.

![esx7 on a legacy m3](https://copydata.tips/wp-content/uploads/2020/07/finale-1024x540.jpg)

But again, this is unsupported by VMware. So please do not bother them if it does not work for you. You will not get any support when using this procedure.

Cheers.