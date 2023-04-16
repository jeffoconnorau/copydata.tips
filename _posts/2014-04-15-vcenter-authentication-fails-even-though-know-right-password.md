---
id: 30
title: 'vCenter authentication fails, even though you know you have the right password!'
date: '2014-04-15T05:05:03+10:00'
author: joconnor
layout: post
guid: 'http://ruste.net/copydata.tips/?p=30'
permalink: /2014/04/vcenter-authentication-fails-even-though-know-right-password/
tags:
    - Authentication
    - SSO
    - vCenter
---

At one of my customer accounts we were experiencing an issue in one of the customer’s sites.

Here is the scenario of the problem.

1\. We Tested Authentication with the AD service account setup for Actifio. Authentication failed, even though we knew we had the right login &amp; details. We also ensured we were using the correct syntax DOMAIN\\Username to login.

2\. Login to the remote vCenter from customers Desktop (which points to a site domain controller in the local site) as the Actifio AD service account. Authentication Works fine, and vCenter is opened and objects displayed.

3\. Test creating a snapshot and creating a new VM from vCenter. All works !

4\. Test Authentication in Actifio with the AD service account. Auth failed again.

But why ? We know we have the correct details, and it can login via vCenter using the same API calls Actifio would make to login.

So, my process of elimination begins. Can we remove the Active Directory as the problem somehow ? Yes, vCenter 5.x and above uses a Single Signon (SSO) process, which can have local accounts. So here is what we tried:

1\. Change the service account in actifio, from the AD service account to be the “admin@system-domain” SSO account (vCenter 5.0 U1), and enter the correct password. Auth Works !

2\. Snapshots work in Actifio, and all is working. Happy Days.

So what does this tell us the root cause is ? My thoughts are it’s one of the following.

a) Possibly time sync is out between the customers remote site vCenter and the remote site Domain controller.

b) The customers remote site Domain Controller is out of sync or not correctly working anymore with Active Directory in the local site.

c) The SSO configuration for VMware and allowing authentication against the customers domain is not setup correctly.

I am leaning towards (c) as the primary candidate for the problem, with potentially (a) too (as we had seen this another time at the customer site).

Either way, this issue is not caused by Actifio or a result of bad code, but it affects us. So knowing how to troubleshoot SSO vCenter authentication is important with the way we authenticate against vCenter in 5.x and above.

Hopefully this saves some time for others to help pin-point the potential issue. The key point is remove AD out of the equation, use the SSO admin account, and see if authentication is still not working. Once the root cause of the AD auth issue in the remote site is debugged, we can then go back to using the limited rights AD service account.