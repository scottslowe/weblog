---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-29T10:30:00Z
tags:
- AWS
- reInvent2017
title: "AWS re:Invent 2017 Keynote with Andy Jassy"
url: /2017/11/29/aws-reinvent-2017-keynote-andy-jassy/
---

This is a liveblog of the re:Invent 2017 keynote with Andy Jassy, taking place on Wednesday at the Venetian. As fully expected given the long queues and massive crowds, even arriving an hour early to the keynote isn't soon enough; there's already a huge crowd gathered to make it into the venue. Fortunately, I did make it in and scored a reasonable seat from which to write this liveblog.<!--more-->

The pre-keynote time is filled with catchy dance music arranged by a live DJ (same live DJ as last year, if I'm not mistaken). There's already been quite a few announcements made this year even before today's keynote: Amazon Sumerian (AR/VR service), new regions and availability zones (AZs), and new bare metal instances, just to name a few of the big ones. There's been a great deal of speculation regarding what will be announced in today's keynote, but there's no doubt there will be a ton of announcements around service enhancements and new services. Rumors are flying about a managed Kubernetes offering; we shall see.

Promptly at 8am, the keynote starts with a brief video, and Andy Jassy, CEO of AWS, takes the stage. Jassy welcomes attendees to the sixth annual conference, and confirms that the attendance at the event is over 43,000 people---wow!

Jassy starts with a quick update on the AWS business:

* $18B revenue run rate
* 42% growth rate (if I captured that correctly)
* Millions of customers with a pretty varied customer base (lots of technology startups, enterprise customers from pretty much every vertical, and public sector users)
* Thousands of system integrators who've built their business on AWS consulting

Jassy reviews the latest "Magic Quadrant," showing AWS with a strong lead over all other competitors, and shows a study that gives AWS 44% of the public cloud marketshare (more than all other competitors combined).

Moving out of the business update, Jassy begins to lay the framework for the rest of the keynote. He compares people building technology solutions ("builders") to musicians, who want the freedom to choose the technology building blocks (the "instruments") to create the solution (the "song"). According to Jassy, AWS radically changes what's possible for builders by giving them unprecedented choice and flexibility. To help with the keynote, a band is going to play five different songs, each of which captures some aspect of how AWS enables builders to build incredibly new and powerful solutions.

The first song is "Everything is Everything," by Lauryn Hill. Jassy explains that "everything is everything" applies to technology because the choice of platform/provider is incredibly important, and builders shouldn't have to settle for less than everything. AWS has more than any other provider, says Jassy, meaning they have the "everything" that builders need/want, leading him into a lengthy rant (in a good way) outlining the breadth of AWS' services (including, notably, a mention of VMware Cloud on AWS).

Jassy mentions that the pace of innovation is also continuing to expand, with an expected 1,300+ service announcements over the course of 2017.

At this point, Jassy brings out Mark Okerstrom, President and CEO of Expedia. Okerstrom talks about the technology challenges that a company operating at Expedia's scale (600M+ site visits monthly, greater than 750 million searches per day) experiences. Expedia has committed to move 80% of mission critical applications to AWS within the next 2-3 years. Why? Resiliency, optimization, and performance, says Okerstrom. Okerstrom wraps up his portion with a quote by Mark Twain (on how travel is fatal to bigotry), and Jassy returns to the stage.

Jassy turns his attention to AWS' compute offerings. Jassy outlines the range of compute instance types (such as the new M5, H1, and I3m [bare metal] instances), and then moves to talk about containers. He positions ECS (Elastic Container Service) as something that AWS built "back when there was no predominant orchestration system," and outlines some of the advantages that ECS offers (deep integration with other AWS services, better scale, and service integrations at the container level).

All that being said, Jassy recognizes that Kubernetes has emerged as a leading container orchestration platform, and that customers who want to run Kubernetes on AWS have some complexities to manage. Jassy recognizes Amazon Elastic Container Service for Kubernetes (EKS), a managed Kubernetes service running on top of AWS. EKS has a number of features that Jassy outlines:

* Hybrid cloud compatible
* Highly available (masters deployed across multiple AZs, for example)
* Automated upgrades and patches

This gives AWS two different managed container offerings: ECS and EKS. However, Jassy says that containers want more---they want to run containers without having to manage servers and clusters. This leads to an announcement of AWS Fargate, which allows customers to run containers without managing servers, clusters, or instances. Just package your application into a container, upload it to Fargate, and AWS takes care of the rest (says Jassy). Fargate will support ECS immediately, and will support EKS in 2018. (Although at this point it's unclear exactly what "supporting" ECS or EKS means.)

Next, Jassy moves on to discussing serverless (Functions as a Service, or FaaS). AWS Lambda has already gathered hundreds of thousands of customers. Jassy points out that FaaS really needs to be more than just code execution; you also need event-driven services (like Lambda and Step Functions), lots of event sources (all the various triggers from AWS services), and the ability to execute functions at the edge as well as in the cloud (like Lambda@Edge and Greengrass).

This brings Jassy back to the "everything is everything" mantra, and how the broad range of compute offerings that AWS supplies satisfies customers' demands for "everything is everything."

Changing direction slightly, Jassy talks about what "freedom" means to him and to AWS. This leads him back to the house band, who plays "Freedom" by George Michael.

The "freedom" discussion leads Jassy to a discussion about databases, and a number of not-very-subtle attacks against Oracle. Customers want open database engines, and this demand is what led AWS to create Amazon Aurora. Aurora is MySQL- and PostgreSQL-compatible but offers the scale and performance that users demand from commercial databases. Jassy states that Aurora is the fastest-growing service in the history of AWS.

Aurora offers the ability to scale out for reads, but customers wanted scale-out write support. Jassy announces a preview of Aurora Multi-Master, which supports multiple instances of Aurora for both read/write support across multiple AZs (with multi-region support coming in 2018). The preview for single region/multi-master is open today.

Next, Jassy announces Aurora Serverless---on-demand, auto-scaling Amazon Aurora. This service eliminates the need to provision instances, automatically scales up/down, and starts up and shuts down automatically.

However, relational databases are the only solution out there; sometimes a different type of solution is needed. Sometimes a key-value datastore is a better solution, leading Jassy to talk about DynamoDB and ElastiCache (which currently supports Redis and Memcached). To expand the functionality and utility of DynamoDB, Jassy announces DynamoDB Global Tables. DynamoDB Global Tables is the first fully-managed, multi-master, multi-region database. DynamoDB Global Tables enables low-latency reads and writes to locally available tables. It's generally available today.

Jassy next announces DynamoDB Backup and Restore, to simplify the process of backing up and restoring data from/to DynamoDB databases. This new offering will enable customers to back up hundreds of terabytes of data with no performance interruption or performance impact. This offering is generally available today, with point-in-time restore coming in 2018.

To better enable using data across multiple databases, Jassy announces the launch of Amazon Neptune, a fully-managed graph database. Neptune supports multiple graph models, is fast and scalable, enables greater reliability with multiple replicas across AZs, and is easy to use with support for multiple graph query languages.

This leads to the third song, "Congregation" by the Foo Fighters. Jassy calls out the apparent contradiction in the song about having blind faith but not false hope, and compares that to the conviction that builders have when building out great ideas even when they're not sure it will work. Getting feedback from customers is one way to help with this, and Jassy says that analytics are the answer here. Naturally, AWS has great analytics, so Jassy talks about the various solutions that AWS has to offer.

In the realm of data lakes, Jassy calls out S3 as the most popular choice for data lakes today, and takes a few minutes to talk about the advantages of S3 (he again refers to the Gartner Magic Quadrant to show S3 in a strong leadership position). S3's position is further strengthened by ties to things like Amazon Athena, Amazon EMR, Amazon Redshift, Amazon Elasticsearch Service, Amazon Kinesis, Amazon QuickSight, and AWS Glue.

At this point, Jassy brings out Roy Joseph, Managing Director at Goldman Sachs, to talk about how Goldman uses analytics on AWS. Joseph stresses Goldman's position as a source of innovation; 25% of Goldman's employees are engineers who have written 1.5B lines of code across more than 7K applications. In order to compete effectively, Joseph says that Goldman Sachs needs strong engineering, risk management, and distribution. Three examples shared by Joseph include Marcus (consumer retail loans), Marquee (access to risk and pricing platform), and Symphony (secure messaging and collaboration; originally internal-only but now seeing growth as an inter-bank platform). So why public cloud? According to Joseph, a greater demand for risk management drives a need for more calculations, which in turn means more compute capacity---and the public cloud was the best way to satisfy that need. That being said, Joseph outlined some concerns that Goldman had to overcome: extending an internally-built management framework and ensuring data privacy. To help ensure data privacy, Goldman worked with AWS to create a "BYOK" (Bring Your Own Key) solution for key management. 

Jassy returns to the stage to continue the discussion around analytics. To help customers perform analytics on the correct subset of data that might be stored in S3, Jassy announces S3 Select, the ability to use standard SQL statements to "filter" out or select the correct subset of S3 data. Jassy shares some TPC-DS benchmarks on a Presto queries (8 seconds without S3 Select, 1.8 seconds [4.5x faster] with S3 Select).

Jassy next announces Glacier Select, which allows you to run queries directly against data stored in Amazon Glacier. This is generally available today.

Shifting focus slightly, Jassy takes the conversation toward machine learning, and asks the house band to play another song. This time it's "Let it Rain" by Eric Clapton, and Jassy says the lyrics of the song reflect the desire of builders for machine learning to be easier to use and embrace than it is right now.

Jassy says that Amazon has been doing machine learning for 20 years, and points to things like Amazon's personalized recommendations, or Alexa's natural language understanding, or the pick paths Amazon uses for the robots in the warehouse. This makes AWS well-positioned to make machine learning easier to use and consume.

According to Jassy, there are three layers to machine learning.

The bottom layer is for expert ML practitioners who deeply understand learning models and frameworks, and Jassy re-iterates AWS' support for all the various major frameworks and interfaces customers want to use.

The middle layer is for everyday developers who aren't experts in ML, but it's still too complicated for most users. To help with the challenges in this layer, Jassy introduces Amazon SageMaker (leverages open source Jupyter project). SageMaker provides built-in, high performance algorithms, but doesn't prevent users from bringing their own algorithms and frameworks. SageMaker also greatly simplifies training and tuning, and helps automate the deployment/operation of machine learning in production.

To further help get machine learning into the hands of developers, Jassy announces DeepLens, the world's first HD video camera with built-in machine learning support. Jassy brings out Dr. Matt Wood to talk more about DeepLens and SageMaker. After talking for a few minutes, Wood does a demo of DeepLens performing album identification and facial expression recognition.

The top layer, according to Jassy, is a set of application services that leverage machine learning. Examples here are Lex, Polly, and Rekognition. Jassy announces Rekognition Video, which is real-time batch video analysis (like what Rekognition does for photos). To help get video/audio data into AWS, Jassy announces Amazon Kinesis Video Streams. Rekognition Video is deeply integrated with Kinesis Video Streams.

On the language side (as opposed to video), Jassy announces Amazon Transcribe to convert speech into accurate, gramatically correct text (initially available with English and Spanish). In the near future, Transcribe will support multiple speakers and custom dictionaries.

Jassy also announces Amazon Translate, which does real-time language translation as well as batch translation. It will support automatic language detection in the near future.

Next, Jassy announces Amazon Comprehend, a fully-managed natural language processing service. It analyzes information in text and identifies things like entities (people, places, things), key phrases, sentiment, and the language of the content. Comprehend can not only identify information in a single document, but can also be used to perform topic modeling across large numbers of documents.

To talk a bit about how the NFL is using Amazon and machine learning, Jassy brings out Michelle McKenna-Doyle, SVP and CIO of the NFL. McKenna-Doyle shares some details on Next Gen Stats (NGS), which spans AWS services like Lambda, CloudFront, DynamoDB, EC2, S3, EMR, and the Amazon API Gateway (among others). NGS generates 3TB of data for every week of NFL games. McKenna-Doyle also talks briefly about future plans for incorporating machine learning and artificial intelligence into the NFL's NGS plans (to do things like formation detection, route detection, and key event identification).

As McKenna-Doyle leaves the stage, the house band kicks up again with another song (remember there are five songs, as outlined by Jassy). This one is "The Waiting" by Tom Petty, and Jassy connects the lyrics to IoT and edge devices.

In order to get out of the keynote in a timely fashion, I'm wrapping up the liveblog here (sorry for the abbreviated coverage).
