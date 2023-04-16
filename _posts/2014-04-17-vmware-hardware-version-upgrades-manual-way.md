---
id: 44
title: 'VMware Hardware Version upgrades &#8211; The manual way.'
date: '2014-04-17T12:27:22+10:00'
author: joconnor
layout: post
guid: 'http://ruste.net/copydata.tips/?p=44'
permalink: /2014/04/vmware-hardware-version-upgrades-manual-way/
dsq_thread_id:
    - '2617830280'
panels_data:
    - 'a:0:{}'
sbg_selected_sidebar:
    - 'a:1:{i:0;s:1:"0";}'
sbg_selected_sidebar_replacement:
    - 'a:1:{i:0;s:7:"Sidebar";}'
tags:
    - VMware
---

<header style="color: #3d3d3d;">The below process is somewhat manual in the process, but it’s been validated a few hundred times by myself personally.

I like to think of VMware’s Virtual Hardware as a virtual motherboard, I remember this explanation was included in the VMware JumpStart materials I used to deliver back in the VI 3.x days. And the Virtual Hardware comes with certain capabilities and restrictions. Newer motherboards are capable of more processors, more memory, more efficient, etc. And commonly, what you normally need to add/remove or replace on a motherboard is the same as Virtual Hardware, in that it often requires a power down to replace or upgrade such items. Yes Hot-Add has changed this somewhat, and definitely for the better, but it’s sometimes an afterthought in many peoples VM templates.

</header><section class="jive-content-body" style="color: #3d3d3d;"><div class="jive-rendered-content" style="font-weight: inherit; font-style: inherit;">Hardware version 4 was available from VMware ESX 3.x and above (pre vSphere branding). With vSphere 4, VMware released hardware version 7 (going from 4 to 7 in one release!). However VMs running hardware version 4 are also backwards compatible with vSphere 4. To update a VM to hardware version 7, the customer must be running vSphere 4.x or above.

The conversion isn’t too hard. but there is about a 5% (my estimate) chance the VM might not boot after the upgrade. It’s irreversible, <span style="font-weight: inherit; font-style: inherit;">unless you </span>perform a VM snapshot prior.

My usual practise when doing this is:

1\. Upgrade VMTools to latest version on the VM. This will normally require a reboot. Even if not prompted, if this is a key VM, do a reboot anyway, and login to the console after to check for any left over driver upgrades that might kick in.

2\. Shutdown the VM (The VM has to be shutdown to upgrade the virtual hardware version — again think of it like a motherboard replacement)

3\. Create a VM snapshot from vCenter (for purposes of rollback), don’t use Actifio CDS for this step as it will likely be a full ingest or low-splash job taking plenty of time, and it will only exist temporarily.

4\. Convert the VM to HW v7 (or 8,9, or 10). Right click on VM, select upgrade hardware version. Confirm you want to proceed.

5\. Power on VM &amp; login to a console session, and check IP address settings are fine. For Windows machines the WINS addresses are generally dropped if the customer is using static WINS Server entries, so re-enter these if they are required. This is one of the reasons doing the HW upgrade can be quite manual, though I would argue that customers should have removed WINS sometime ago, but sometimes you just can’t control the customer environment, so I noted the step here as it can be the difference between a successful upgrade or rollback.

6\. Restart the VM (this step is important as the hardware upgrade is the same process as a motherboard upgrade) and upon first boot new hardware is often found and plug and play kicks in. Having the latest VMware Tools installed prior to this step is key.

7\. Once the VM is restarted, login to the VM console, check all services are started upon boot, and IP addressing is still in place, DNS Search suffixes, WINs, etc.

8\. If everything looks OK, you can then delete the VM snapshot from vCenter, which <span style="font-weight: inherit; font-style: inherit;">can</span> be done online without requiring the VM to be shutdown.

You’re VM is then running hardware version 7/8/9/10 or whatever is the highest version available. There are other methods to do this, and some people skip the step of taking the snapshot, and it can be done quicker. But the above method offers rollback, but unfortunately 3 reboots which is a pain. Even if the customer is on vSphere 4.x and hasn’t upgraded the virtual hardware version, there are performance benefits for doing this on top of changed block tracking benefits.

Below are the hardware versions which came out with the release mentioned.

HW v4 = ESX/ESXi 3.5 and above

HW v7 = ESX/ESXi 4.0 and above

HW v8 = ESXi 5.0 and above

HW v9 = ESXi 5.1 and above.

HW v10 = ESXi 5.5 and above.

Your customer is hopefully not confused that HW v4 means vSphere 4, as this is not the case. And also remember these upgrades are irreversible once the snapshot is deleted, and the more recent versions need to be all done via the vSphere Web GUI.

</div></section>