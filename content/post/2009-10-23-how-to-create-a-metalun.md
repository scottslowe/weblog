---
author: slowe
categories: Tutorial
comments: true
date: 2009-10-23T21:48:45Z
slug: how-to-create-a-metalun
tags:
- EMC
- Storage
title: How To Create a MetaLUN
url: /2009/10/23/how-to-create-a-metalun/
wordpress_id: 1704
---

MetaLUNs are a way of expanding a LUN for either additional I/O capacity (using a striped MetaLUN) or additional space (using a concatenated MetaLUN). A _striped MetaLUN_, as the name implies, stripes data across multiple component LUNs. Each of these component LUNs resides on a different RAID group, so creating a striped MetaLUN allows the MetaLUN to utilize all the spindles in all the RAID groups. A _concatenated MetaLUN_, on the other hand, fills up one component LUN before moving on to the next component LUN. I/O capacity is essentially unchanged, but storage capacity is expanded.

[This article](http://www.emcstorageinfo.com/2008/03/type-and-benefits-of-meta-lun.html) has some good information on MetaLUNs.

While trying to learn more about MetaLUNs, I searched high and low for a "how to" guide on creating MetaLUNs. Surprisingly enough, I didn't find anything. So, here's my take on how to create a MetaLUN. EMC experts, feel free to refine my process, correct my errors, and provide general guidance.

The equipment used in my experimentation is a CLARiiON CX4-960 running FLARE 28 (as best I can tell). I performed this task using Navisphere 6 running under Internet Explorer 6 on Windows Server 2003.

OK, ready? Here we go:

1. Create the necessary RAID groups to house multiple LUNs. Based on my limited experience thus far, it seems like RAID 5 (4+1) is the most common configuration for a RAID group unless you know you need something else.

2. Create LUNs _with the exact same size and settings_ in each of the RAID groups you created. For maximum performance, ensure that the LUNs are created on separate buses. I used LUNs on three separate RAID groups: one on Bus 0, one on Bus 1, and one on Bus 2.

3. Right-click on the first LUN you created and select Expand.

4. Click Next to start the wizard.

5. Select Striping and click Next.

6. Select each of the LUNs. Hold down the Control key to select more than one LUN. Click Next when you have selected all your component LUNs.

7. Make sure Maximum Capacity is selected and click Next.

8. Click Finish.

When the wizard is done processing, the LUNs you created in the RAID groups will be moved into the Private LUNs container, and a new MetaLUN object will appear in Navisphere (under LUN Folders > MetaLUNs). You can then present this MetaLUN out to one or more servers in the same way you would present any other LUN object.

If I've misrepresented something or have provided incorrect information, please let me know by speaking up in the comments. I'd also love to hear any recommendations from EMC experts on the use of MetaLUNs, advantages and disadvantages of using them, etc. Share your knowledge!
