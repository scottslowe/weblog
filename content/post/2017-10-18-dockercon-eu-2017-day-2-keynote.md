---
author: slowe
categories: Liveblog
comments: true
date: 2017-10-18T10:30:00Z
tags:
- Docker
- DockerCon2017
- IBM
title: DockerCon EU 2017 Day 2 Keynote
url: /2017/10/18/dockercon-eu-2017-day-2-keynote/
---

This is a liveblog of the day 2 keynote/general session here in Copenhagen, Denmark, at DockerCon EU 2017. Yesterday's keynote (see [the liveblog here][xref-1]) featured the hotly-anticipated Kubernetes announcement (I shared [some thoughts here][xref-2]), so it will be interesting to see what Docker has in store for today's general session.<!--more-->

At 9:02am, the lights go down and Scott Johnston, COO of Docker ([@scottcjohnnston on Twitter][link-1]), takes the stage. Johnston provides a brief recap of yesterday's activities, from the keynote to the breakout sessions to the party last night, then dives into content focusing around modernizing traditional applications through partnerships. (If two themes have emerged from this year's DockerCon EU, they are "Docker is a platform" and "Modernize traditional applications".) Johnston shares statistics that show 50% of customers have leveraging hybrid cloud as a priority, and that increasing major release frequency is also a priority for enterprise IT organizations. According to Johnston, 79% of customers are saying that increasing software release velocity is a goal for their organizations. Continuing with the statistics, Johnston shows a very familiar set of numbers stating that 80% of the IT spend is on maintenance (I say familiar because these numbers were also used by VMware---and other vendors, no doubt---in years past). This leads Johnston to a discussion of the Modernizing Traditional Applications (MTA) Proof of Concept (POC) Program (the MTA POC Program, or the MTAPOCC---acronyms, anyone?). Based on the results of the MTA POC Program, Johnston shares that Docker (and their partners, who thus far remain unnamed) has been successful in modernizing (containerizing) 100% of applications. He proceeds to share a story of containerizing a 2005-era .NET 2.0-based application successfully in Docker. Johnston states that the program has proven successful in making appliations more "hybrid cloud ready," increasing software release agaility, and improving the security of applications (via isolation and integry, through digital signing).

Johnston now brings out Ashwini Oruganti and Riyaz Faizullabhoy to do a demo that illustrates some of the concepts that Johnston just discussed. Oruganti and Faizullabhoy walk through a series of security checklist items and show how Docker EE satisfy each of the requirements. The various features include encrypted networks, appropriate role-based access control (RBAC), and digital signatures on container images. Toward the end of the demo, Kristie Howard (from yesterday's demos) makes a brief appearance, then the demo ends.

Johnston returns the stage following the demo to talk about how Docker's MTA POC Program offers "revolutionary results" through an "evolutionary approach". He then brings out Iain Gray and Brandon Royal to talk a bit about Docker's approach to modernizing traditional applications.

Gray and Royal recap the properties of the MTA approach: incremental, non-disruptive, and customer-driven. The MTA approach starts with a proof-of-concept, followed by quickly putting those early PoC apps into production, and then scaling the applications (and the volume of modernization, I assume) at the customer's pace.

Diving a bit deeper into the process, Gray reviews that the PoC portion of the MTA approach starts with an assessment step. The assessment step identifies candidate applications (which are typically Linux/Java or Windows/.NET applications) and defining the success criteria. Once candidate applications have been identified and success criteria defined, customers can proceed with containerizing the application, deploying the application, and finally going into a measurement phase. In the measurement phase, it's about measuring the success criteria and verifying they were satisfied/achieved, as well as building a return-on-investment (ROI) model to show benefits of continuing the MTA process.

Royal takes over now to talk about the second part of the MTA approach (moving the first applications into production), which itself has a number of phases (assessing, containerizing, operationalizing, test and acceptance, going live, and finally measuring/learning/closing the feedback loop). In addition to the different phases listed, this second step of the MTA approach also involves building a foundation for building and operating modernized applications. According to Royal, this involves establishing appropriate governance (internal SLAs, training, documentation), deploying the platform (deploying Docker EE, integrating with other systems), and creating a toolchain (CI/CD pipelines, Content Trust, Security Scanning). Royal repeats that while containerizing the application is important, it's about building a repeatable process and framework. (In my view, this is the hard part.)

Gray takes over from Royal to talk about the third step in the MTA approach, which involves scaling applications and scaling the MTA process (now that a repeatable framework has been established). Gray indicates that as the MTA process scales within an organization, the foundation Royal discussed will evolve and grow. The final step is innovating at a customer-driven pace; here, Gray mentions things like refactoring applications, moving to a microservices-based architecture, or deploying to the cloud (or hybrid cloud).

At this point, Gray brings out Markus Niskanen and Ocas Renalias (From Finnish Rail and Accenture, respectively) to provide a real-life example of how Finnish Rail has embraced containers and Docker to modernize/containerize their traditional applications. Some of the drivers pushing Finnish Rail were cost, speed, and quality (the same factors that have been mentioned by others numerous times in this keynote presentation).

Returning to the stage, Johnston reviews the importance of partners in the MTA journey. Partners provide guidance, choice, and innovation. This leads to an announcement from Johnston that IBM is joining the MTA Program. Johnston brings out Jason McGee from IBM Cloud Platform to talk more about the value that IBM brings to the Docker MTA Program. McGee spends a few minutes talking about the length and maturity of the relationship between IBM and Docker and the value IBM brings to Docker, the Docker MTA Program, the Docker community, and Docker customers. McGee also shares a few announcements: IBM is now Docker Certified and will have IBM software in the Docker Store; and Docker for IBM Cloud (similar to Docker for AWS or Docker for Azure). Along the way, McGee does a demo of Docker for IBM Cloud. It's actually a pretty good demo, showing how to integrate applications with IBM services like Watson.

Johnston now comes back to the stage following IBM's presentation and demo to thank all of Docker's MTA Program partners. Looking ahead, Johnston says that most of the requests from customers regarding modernizing (containerizing) applications come in two major categories: support for more application types (C/C++, COBOL, etc.), and more automation tools (discovery, dependency mapping, ROI, etc.).

In closing, Johnston reviews some breakout sessions that will provide more information and/or insight on the topics covered in the general session, and then closes the session.



[link-1]: https://twitter.com/scottcjohnnston
[xref-1]: {{< relref "2017-10-17-dockercon-eu-day-1-keynote.md" >}}
[xref-2]: {{< relref "2017-10-17-some-thoughts-on-docker-kubernetes-announcement.md" >}}
