---
id: 203
title: '5 Questions I Ask Every Customer about their VMware Backup Strategy'
date: '2019-04-12T09:54:14+10:00'
author: joconnor
layout: revision
guid: 'https://copydata.tips/2019/04/198-revision-v1/'
permalink: '/?p=203'
---

I can’t remember of a technology platform that provided better APIs than VMware’s VADP API framework. While it has had annoying bugs periodically, overall the APIs made it extremely simple, easy, and efficient for VMware VM backups.

It’s no wonder, there are a huge amount of backup product vendors that all claim features like application consistent and incremental forever backup (using VMware Change Block Tracking (CBT)) and instant recovery of VMs. There really is very little competitive differences now between these products! All 25+ vendors are all calling the same library for CBT capture.

But what really matters in terms of costs and RTO is where and how you store that backup data and metadata. In this cloud era, it’s hard not to consider the cloud as a target to store backups and reduce data centre footprint and costs for backup and DR.

Here are 5 simple questions I ask all of my current and prospective customers to think about when thinking about their VMware backup strategy.

> **1. Why have local on-premises backup copies? Why not back it up directly to the Cloud?**

*Not all data is born equal*.

Multiple studies have shown that it’s important to tier your VMs and then apply backup and retention policies. So for all the Tier-2 VMs, which typically constitute anywhere between 40% to 70%, what if you could eliminate the local copy and backup directly to cloud object storage like AWS S3, S3IA, Azure Blob, Google Nearline, IBM COS?

They all offer 11 x 9s of durability in three availability zones. It costs less. There is no capacity management as you don’t have to scramble for storage when you add the next 100 Tier-2 VMs for backups. There is zero operational burden with this approach.

Obviously, this is not an ‘all or none’ approach. For your Tier-1 VMs, you might still have a requirement or bandwidth constraint so you can choose to have a local cache/backup copy in your data center, and also have a second backup copy in such a cloud object storage.

> **2. How long are your restores taking today?**

*Is that acceptable as your data grows…*

Obviously, if you like the approach of [leveraging cloud object storage](https://www.actifio.com/company/blog/post/backup-to-the-cloud-object-storage-actifio-vs-others/), your next thought would be “What about the recovery time objective (RTO)?”

Most backup products, unfortunately, take a long time to recover from their deduplicated backups stored in cloud object storage. The catch cry for some reason is still around the “backup industry”, but I’ve been calling out that the priority is wrong, it should be called a “recovery industry”. We backup so we can recover! That is what a business is really after when it invests in a backup solution, and more often they can’t afford to wait days and hours to get their critical data back from a dedup engine or tape.

Object storage can really help solve two pain points there, it’s infinite in scale, and yet very quick to mount the data back. Couple it with a next-generation backup product that writes the data in its native application format, and you’re starting to fix a lot of the legacy issues from a backup mindset, versus a recovery mindset! Recovering that 10TB VM or SQL/Oracle Database is just a few minutes away now, It’s a game changer people… seriously!

> **3. If you are restoring VMs from the cloud, are you concerned about the egress costs?**

*Optimise every bit that moves***.** One of the concerns enterprises express is the egress charges from the cloud back to on-premises. Let’s explore, with an example, of how much would it cost you on a monthly basis.

Let’s assume you have 1000 VMs that are being protected. Let’s assume, on an average 20 restore jobs for files/folders are performed per week, i.e. 80 restore jobs a month. Assume that on an average 100 MB of files are restored in each job. This translates to 80 x 100 MB = 8GB of total data restored from the cloud.

Assuming you use AWS S3 IAS (Infrequent Access Storage), it charges $0.01 per GB. The data retrieval charges = $0.08 per month. AWS also charges for the data that leaves AWS cloud at a rate of $0.09 per GB. This translates to $0.72 per month. Thus total costs = $0.08+$0.72 = $0.80 per month, which obviously is very low.

Now let’s look at a scenario where 20 VMs are recovered from cloud object storage back to on-premises. Assume average VM size = 200GB. Thus total data transferred = 20 x 200GB = 4,000 GB. Thus total data transfer charges = ($0.01+$0.09)\*4000GB = $400 for the entire month.

The good news is that even this small monetary amount can be reduced further. Consider a next-generation approach, where not all data needs to be recovered if it’s already on-premises. Features like “delta block differencing” technology will lookup its metadata to compare which blocks already exist in the local backup cache on-premises and transfer only those blocks from the cloud which don’t already exist on-premises. So in the above example, if you assume 40% of the blocks already exist on-premises, only 2,400 GB will be copied from the cloud, thus reducing the data transfer costs to $240.

> **4. Do you require any data immutability capability at the software and cloud storage layer?**

*Was it a fat-finger or a rogue internal user?*

One of the legit concerns enterprises have is that of a rogue or malicious user who could potentially delete backups.

What if at the software layer, an admin can apply a data immutability lock on backups of specific VMs. Once this is applied, even an admin can not expire or purge the backups for those specific VMs.

You can still manage the TCO for disk by setting an expiration date for the backup data, as per the original required policy, or elect to never expire it, yet not be concerned about a rogue admin or a fat finger.

> **5. Do you like buying hardware appliances? Why not software only, or a SaaS platform?**

*“The world we have created is a process of our thinking. It can not be changed without changing our thinking.” — Einstein*

Many enterprises are getting used to “as-a-Service” consumption model with exposure to GSuite, Office 365, Salesforce, Github, AWS RDS &amp; Redshift and the likes of VMware Cloud in AWS.

So they are also questioning the idea of purchasing hardware appliances for everything, not only backup appliances but also minimising their production compute platforms too. Why not consider a VMware Backup SaaS platform? Why not consume VMware backup and recovery to cloud in a simple per VM subscription pricing model?

In my earlier days at Actifio, we only sold a hardware solution, and many customers pushed back, asking for a software appliance version. We responded to those customers and launched Sky into the marketplace, and it’s been very successful to say the least. But we’re now going one step further and offering a SaaS version for VMware backup to AWS/Azure/Google/IBM cloud. Check out more details here at [ActifioGo.com](https://actifiogo.com/). The only consideration to factor in is some good bandwidth to your local cloud and you can be off and running in a few minutes.

Every few years, technology advances enable new capabilities which enable us to question some of the existing practices/assumptions and adjust appropriately. Examples like API-driven IaaS and PaaS were deemed too complex and resource intensive to automate. But the IT world has shifted, and enterprises are all looking at the benefits of cost optimisation, automation, and orchestration across their entire stack. I’d highly recommend you consider asking yourself the 5 questions above, and see how you can benefit from the likes of a next-generation platform combined with object storage, with 11 x 9s of durability, almost infinite capacity, and pay-as-you-grow models, which simplifies your life and lets you focus on other important tasks.

Check out this [short 3-minute video](https://www.youtube.com/watch?v=e5hNCtb-Bk0) by my good mate Chandra Reddy which compares architectures to leverage cloud object storage effectively. It’s well worth the time.

Feel free to reach out to me, if you want to discuss or dispute any of what I have covered above.