---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-18T18:09:30Z
slug: po2061-vmware-virtualcenter-25-database-best-practices
tags:
- Virtualization
- VMware
- VMworld2008
title: 'PO2061: VMware VirtualCenter 2.5 Database Best Practices'
url: /2008/09/18/po2061-vmware-virtualcenter-25-database-best-practices/
wordpress_id: 947
---

This is PO2061, VMware VirtualCenter 2.5 Database Best Practices. The presenter is Bruce McCready with VMware. My battery is starting to run low, so I may lose some or all of the rest of this session partly through the session. I have another battery that I can swap in if necessary.

The session starts out with the standard disclaimer. Most of this stuff is already out there, but the disclaimer is required anyway.

The VirtualCenter database is clearly an important part of the overall virtualization implementation. The goals of this presentation will be to help users get a better understanding of the VC DB inner workings, to help users take advantage of changes in VC 2.5 and VC 2.5 Update 1, talk about performance statistics and how they affect DB performance, and answer some FAQs regarding VC DB size and performance.

As an overview of the VC DB, it stores host and VM details. The DB is slightly larger in VC 2.5 as it keeps about 40% more data about hosts. The VC DB also stores information about alarms and events. Old data about alarms and events is _not_ purged over time. Finally, the VC DB also stores performance statistics. Up to 90% of the DB is taken up by performance statistics. Performance statistics are the primary factor in scalability and performance of the VC DB, which is what led to a major refactoring of the database in VC 2.5 as opposed to VC 2.0.

Bruce briefly describes the components that add load to the VirtualCenter database: performance statistics inserts, performance statistics rollup stored procedures, and ad hoc queries.

There are four levels of performance statistics. Level 1 has only about 10 statistics, Level 2 goes to 25 items, Level 3 ups that to about 65, and Level 4 increases to around 100 items. You can multiply the number of statistics being captured by the number of objects in inventory to get a rough idea of how much data is being inserted into the VC DB.

Stored procedures are run as scheduled jobs in SQL Server every 30 minutes, 2 hours, and 24 hours. Every half hour the 5 minute statistics are rolled up into the half hour roll up. Every 2 hours the half hour statistics are rolled up, and every day the two hour statistics are rolled up. After a statistic is a year old, it is automatically purged. Some statistics (like the 5 minute statistics) are kept for shorter periods of time.

Noting that performance statistics are purged periodically, it's important to understand the most of the unchecked table growth comes from alarms and events.

The performance improvements in VC 2.5 allow the VC DB and stored procedures to scale much higher and greatly reduce the amount of time required for the operations to complete. In addition, the refactoring also allowed the VC 2.5 database to reduce the amount of space required to store the same amount of performance data.

Bruce next shows some performance results about response times for various operations (rollup, purge, and insert).

When it comes to sizing for optimal performance, memory is the most important thing. Disk I/O and CPU are important, but memory will most likely have the greatest impact. SQL Server's TEMPDB isn't as important in VC 2.5 as it was used in VC 2.0. Keep in mind that SQL Server will take advantage of multi-core situations, and the VC DB is optimized to help take advantage of SQL Server's parallelism.

Bruce again shows how more memory will greatly reduce the time required to perform two-hour rollups on a very large inventory with Level 4 performance statistics. This just reinforces the need to give VC plenty of memory.

To help calculate the sample size of performance statistics, use this formula:

	Sample size = (number of entities) * (statistics per level)

Based on the sample size, VMware has come up with some recommendations for VC DB hardware:

Up to 40K: 1 core, 1GB of RAM, less than 120 IOPS  
40K to 80K: 2 cores, 2GB of RAM, 120-200 IOPS  
80K to 120K: 4 cores, 4GB of RAM, 200-300 IOPS  
120K to 160K: 4 cores, 6GB of RAM, 400-500 IOPS  
160K to 200K: 6 cores, 8GB of RAM, 500-600 IOPS  
More than 200K: 8 cores, 12GB of RAM, 650 IOPS

Next he moves on to some performance tips for VC 2.5. One trick to configure different collection levels for different time periods. For example, longer-term data can be kept at Level 1, but short-term data can be gathered at Level 3.

From the SQL Server perspective, it may be beneficial to separate the data and log onto different physical drives. In addition, monitoring page fullness in vpx_hist_stat1 and vpx_hist_stat2 may also help improve performance. Use the NOLOCK option for ad hoc queries to prevent blocking of other operations that need to occur, and correctly size the memory that is allocated to SQL Server. In addition, configuring parallel processing in SQL Server may be helpful. This is typically on by default, but if it is off then you may want to turn it on.

It is important to decide on a suitable backup strategy as well.

Enable vardecimal format for vpx_hist_stat can reduce database size about 60%. However, vardecimal format is only supported on SQL Server 2005 SP2, and only in certain editions of that version (such as the Enterprise Edition).

Purging old data can also help with DB performance. Remember that performance statistics are purged automatically, but alarm and event data is not. See VMware KB article 1000125.

The presentation is still going and looks like it has about 20 minutes left at the most, but the battery is running low now so I'm going to go ahead and publish this entry. Readers who attended this session are invited to flesh out my notes in the comments for additional information that I wasn't able to cover.
