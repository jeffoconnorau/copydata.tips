---
id: 169
title: 'vSphere ESXi6.0 CBT (VADP) bug that affects incremental backups / snapshots.'
date: '2015-11-14T08:31:49+11:00'
author: joconnor
layout: revision
guid: 'http://ruste.net/copydata.tips/2015/11/167-revision-v1/'
permalink: '/?p=169'
---

VMware recently posted a new KB article 2136854 to advertise the issue. Which is great that this has finally been accepted and advertised to customers and partners.

It’s important to know this is not the same one as posted recently also for ESXi 6.0 (KB 2114076) – now fixed in a re-issued build of ESXi 6.0 (Build 2715440)

But it is very similar to KB 2090639 from a historical perspective.

<span style="text-decoration: underline;">**The Issue**</span>

If you are leveraging a product that uses VMware’s VADP for backup, then chances are you are leveraging this for not just initial fulls, but regular incremental snapshots (for backup purposes). There are numerous products on the market that leverage this API, it’s virtually the industry standard to use this feature as it results in faster backups.

When the incremental changes are being requested through the API (QueryDiskChangedAreas) the API is requested changed blocks, but unfortunately some of the changed blocks aren’t being correctly reported in the first place, so backup data is essentially missing. And backups based on this can be inconsistent when recovered and result in all sorts of problems.

<span style="text-decoration: underline;">**The Challenge**</span>

Currently there is no resolution or hotfix to the issue from VMware. I hope that we will see something in the coming days due to the wide ranging impact to customers and partner products affected.

<span style="text-decoration: underline;">**The Workarounds**</span>

The workarounds in the KB suggests:

1. Do a full backup for each backup, and that will certainly work, but it’s not really a viable fix for most customers
2. Downgrade to ESX 5.5 and virtual hardware back to 10 (ouch !)
3. Shutdown the VM before doing an incremental

From the testing we have done at Actifio, option 3 doesn’t actually provide a workaround either, and options 1 &amp; 2 aren’t really ideal either.

<span style="text-decoration: underline;">**The Discovery**</span>

When Actifio Engineers discovered the issue, we contacted VMware and proved the problem leveraging just API calls to demonstrate where the problem was. How did we discover the issue I hear you ask ? Well we managed to discover the issue via our patented fingerprinting feature that occurs post every backup job. This technique (feature) essentially has learnt to not trust the data we receive (history has proven this feature to be useful many times) but to go and verify it against our copy and the original source copy. If we receive a variance in anyway, we trigger an immediate full read compare against the source and update our copy. This works like a Full Backup job, but doesn’t write out a complete copy again, it just updates our copy to line up with the source again (as we like to save disk where we can!). We’ve seen this occur from time to time with our many different capture techniques (not just VADP), so it’s a worthy bit of code to say the least that our customers benefit from.

Let’s hope theres a hotfix on the near horizon, so the many many VADP / CBT vendor products that rely on it, can get back to doing what we do best and that’s protecting critical data for our customers that can be recovered without question.

Cheers