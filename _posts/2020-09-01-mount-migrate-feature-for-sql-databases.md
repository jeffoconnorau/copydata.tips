---
id: 246
title: 'Mount &#038; Migrate feature for SQL Databases'
date: '2020-09-01T01:16:00+10:00'
author: joconnor
layout: post
guid: 'https://copydata.tips/?p=246'
permalink: /2020/09/mount-migrate-feature-for-sql-databases/
sbg_selected_sidebar:
    - null
sbg_selected_sidebar_replacement:
    - null
tags:
    - SQL
---

A while back I created this short demo video, using a real world sized SQL database (1TB) to highlight the rapid recovery options that Actifio 10c enables for customers.

This feature is way ahead of the market. But in brief it does these things.

1. Allow customers with large (in fact any size, but the larger the size the better this feature is…) to get mission critical DBs back online in a very low RTO capability. This gets your business applications back online quick.
2. It then solves the problem of running the restore of the Database back to the location where you now want the DB to run from.
3. It then continually restores the changes that are being made to the mounted database, and trickling those changes to the local files, in preparation for the switch to take place, from a mounted DB to a fully local DB.
4. Performs a final switch process, where the DB is then temporarily taken offline, while the final changes are backed up and restored, and then the Database is switched over to the local copy in a switch over that should take just a few seconds to complete.

The final switch over process can also be done at any point the DBA is ready. Providing the DBA with a solution, that rapidly gets their DB online in seconds, runs a restore while the DB is online, then switches over when ready to get back to the production disk location.

Super cool, and again… way ahead of the pack for mission critical SQL databases.

<iframe allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen="" frameborder="0" height="489" loading="lazy" src="https://www.youtube.com/embed/wD2NT_vb6M0?feature=oembed" title="Actifio 10c TechTip highlighting our game changing Mount and Migrate feature." width="870"></iframe>