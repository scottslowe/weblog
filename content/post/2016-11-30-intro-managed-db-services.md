---
author: slowe
categories: Liveblog
comments: true
date: 2016-11-30T17:30:00Z
tags:
- AWS
- reInvent2016
title: 'Liveblog: Introduction to Managed Database Services on AWS'
url: /2016/11/30/intro-managed-db-services/
---

This is a liveblog of the AWS re:Invent session titled "Introduction to Managed Database Services on AWS" (DAT307). The speakers for the session are Steve Hunt, Alan Murray, and Robin Spira, all of FanDuel; and Darin Briskman, from AWS Database Services.

Briskman kicks off the session with a quick review of AWS' managed database offerings. These fall into four categories, which Briskman reviewed so quickly I couldn't capture. I think they were SQL, NoSQL, data warehousing, and something else. Why use managed databases? Because this allows AWS to take over the responsibility for OS maintenance, DB maintenance, high availability, scalability, etc. All you have to worry about it is the application that runs on the database.

What are the managed relational database services that AWS offers?

* _Amazon RDS (Relational Database Service):_ The oldest service, now supporting MySQL, MariaDB, PostgreSQL, Microsoft SQL Server, and Oracle
* _Amazon Aurora:_ MySQL-compatible (and now PostgreSQL-compatible per the announcement today) with greater scalability, better performance, transparent encryption, high availability, and integration with AWS Lambda

Relational databases are really helpful in many cases, but sometimes NoSQL databases would be more helpful. AWS also offers DynamoDB, which is a managed NoSQL database service. DynamoDB is always clustered, and automatically shards/partitions as needed based on size and/or throughput.

Amazon ElastiCache leverages one of two engines---either Memcached or Redis---for high-speed in-memory key/value stores. Like the other database offerings, it's fully managed. You can use ElastiCache as a primary data store, but it is more commonly used as a cache in front of other database offerings (managed or unmanaged).

For analytics in a data warehousing environment, Amazon offers Amazon RedShift. RedShift is a PostgreSQL-based solution. It's massively parallel and operates at a petabyte scale. Again, it's fully managed by Amazon.

Also in the analytics space is Amazon Elasticsearch Service, which is a managed service with Elasticsearch and Kibana. You could use this to pull in CloudWatch and CloudTrail logs and visualize them using this service.

Finally, there's Amazon EMR. This includes MapReduce, Spark, and Presto, and is a managed service that allows customers to pull data from S3, HDFS, or MapR to do analysis without having to worry about the details of managing and optimizing a cluster. One popular use pattern has organizations employing spot instances with Amazon EMR.

At this point, Briskman turns the stage over to Robin Spira from FanDuel. FanDuel offers online fantasy sports (think fantasy football). After a brief introduction, Spira hands it over to Alan Murray. Murray shares some additional information about FanDuel's growth and the challenges that resulted from that growth.

In the first generation version of the application, it was a LAMP stack with PHP code running on MySQL. Challenges faced here led FanDuel to re-architect to the next-generation architecture.

In 2010, FanDuel migrated to a cloud provider (not AWS); issues experienced there forced FanDuel to migrate to AWS in 2011. The web server layer was refactored to enable some level of horizontal scaling, but the monolithic database persisted.

In 2011, FanDuel migrated entirely to AWS, but scaling issues due to application architecture persisted.

In 2012, FanDuel moved the architecture toward SOA, introducing message queues to decouple services. FanDuel started to use Puppet to help with configuration management, and they started evaluating the use of Elastic Load Balancers (ELBs) and Auto Scaling Groups (ASGs) to help with scaling. However, the monolithic database remained, and was now running on one of Amazon's largest EC2 instances (but was still running hot).

2013 saw the introduction of a REST-based API layer (to help support a new iOS mobile client), continued work on the back-end services, but FanDuel (along with many other daily fantasy sports [DFS] vendors) saw a major outage. This led to immediate work on improving FanDuel's scalability and DR position.

By 2014, FanDuel's business volumes were now >3,000x the original volume. The core monolithic database survived the season, but FanDuel did see a Redis degradation failure.

In 2015, business volume soared another order of magnitude, the road to SOA continued, and FanDuel moved from a self-managed MySQL instance to a MySQL RDS instance. This made database-related tasks far easier by pushing them off to Amazon.

Where is FanDuel's application now? The service is pretty reliable, and performance is pretty good with headroom to grow yet. FanDuel remains on a MySQL RDS instance, not yet having migrated to Aurora or DynamoDB. (Murray did not indicate whether they are using ElastiCache Redis instead of a self-managed Redis cluster.)

Murray spends the next few minutes going into a more detailed discussion of FanDuel's application and some of the specific challenges involved in providing their DFS applications/services. He also shares a number of graphs that depict the volumes of entries, lineups, viewing events, live scoring events, etc., with which FanDuel's application must deal.

Murray finally gets to the point where he discusses FanDuel's early experiences with Aurora. Moving to Aurora completely removed the replication lag they'd been seeing with read replicas, and performance generally improved by 3x to 5x. Murray also briefly discussed the move from self-managed Redis to ElastiCache Redis. The key benefits were greater operational simplicity and faster master failover.

At this point, Murray hands over to Steve Hunt to discuss the migration(s) that FanDuel experienced along the way. This was a pretty straightfoward, if time-consuming process, using standard MySQL tools like `mysqldump` to dump the data from the source and import it into the target, setting up replication to re-sync the source and target, verifying consistency, demote source/promote target, and then switch the application.

Murray now takes the stage again to quickly summarize FanDuel's experience with managed database services on AWS. Those benefits include automated failover, managed backups, no more replication lag with read replicas, common interfaces and tooling, and operational cost reduction.

Murray hands the stage back over to Briskman, who quickly wraps up the session (which is already running late).
