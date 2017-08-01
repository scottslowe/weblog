---
author: adelp
categories: Education
comments: true
date: 2009-05-11T12:00:42Z
excerpt: I wanted to follow up on my previous article on the Intel Nehalem CPUs with
  some additional information on memory configurations.
slug: introduction-to-nehalem-memory
tags:
- Hardware
- Nehalem
- Intel
- GuestPost
title: Introduction to Nehalem Memory
url: /2009/05/11/introduction-to-nehalem-memory/
wordpress_id: 1339
---

_By Aaron Delp_

I wanted to relay some information regarding choosing memory speeds and types for the new Intel Xeon 5500 (Nehalem family) processors. As stated in my previous [article on the Nehalem CPUs][1], there are some decisions that need to be made when choosing the memory and processor combinations. Lets start off with what the memory architecture looks like.

* The current Xeon 5500 family is a two-socket configuration.

* Memory will run at 1333 MHz, 1066 MHz, and 800 MHz.

* Memory is currently produced in single, dual, and quad rank configurations. Dual rank is faster than single rank, quad rank is currently limited to 1066 MHz speed.

* Each CPU socket has 3 memory channels for a total of 6 channels per server.

* Each **channel** can accept up to 3 DIMMS. This is why the servers currently are made with either 12 sockets (2 DIMMS per channel x 3 channels per processor x 2 processor sockets) or 18 sockets (3 DIMMS per channel x 3 Channels per processor x 2 processor sockets).

* Some servers come in a 16 DIMM arrangement. Please see [this IBM Paper](ftp://ftp.software.ibm.com/common/ssi/sa/wh/n/xsw03025usen/XSW03025USEN.PDF) for more information.

* The maximum memory speed is limited by processor. For example, the X5570 has a max memory speed of 1333 MHz, the E5540 has a max memory speed of 1066 MHz, etc.

* As more memory is added to a **channel**, the memory will slow down.

* Better performance is achieved when the memory is balanced (the total amount of memory across channels is the same).

Take a look at the [Hp Quick Specs for the BL460 G6 server in the Memory section](http://h18000.www1.hp.com/products/quickspecs/13238_na/13238_na.HTML#Memory). I found this to be a great source.

So, what does all of that mean? It means that for best performance you should install the memory using the following guidelines:

* Ideally, install DIMMs in sets of 6, 1 per channel (populate both sockets with CPUs!). Use DIMMs that are dual rank and have the fastest speed you can purchase that the processor supports.

* Populate the first slot in all channels first, then populate the 2nd slots in all channels, etc. Dont put all three DIMMs in one channel and leave other channels empty.

* Balance the amount of memory in each channel whenever possible (3 x 4GB on two channels and 1 x 4GB 1 X 8GB on the last channel).

* If at all possible, try to keep the system away from the 800MHz memory speed.

Here is link to an _awesome_ [IBM white paper](ftp://ftp.software.ibm.com/common/ssi/sa/wh/n/xsw03025usen/XSW03025USEN.PDF) explaining everything.

Here's an example 12 DIMM slot Nehalem configuration:

| CPU (Speed)     | Max Mem   | Bank 1    | Bank 2    |
|                 | Speed     | Populated | Populated |   
|-----------------|-----------|-----------|-----------|
| X5570 (2.93GHz) | 1333 MHz  | 1333 MHz  | 1066 MHz* |
| X5560 (2.80GHz) | 1333 MHz  | 1333 MHz  | 1066 MHz* |
| X5550 (2.66GHz) | 1333 MHz  | 1333 MHz  | 1066 MHz* |
| E5540 (2.53GHz) | 1066 MHz  | 1066 MHz  | 1066 MHz  |
| E5530 (2.40GHz) | 1066 MHz  | 1066 MHz  | 1066 MHz  |
| E5520 (2.26GHz) | 1066 MHz  | 1066 MHz  | 1066 MHz  |
| E5506 (2.13GHz) | 800 MHz   | 800 MHz   | 800 MHz   |
| E5504 (2.00GHz) | 800 MHz   | 800 MHz   | 800 MHz   |
| E5502 (1.66GHz) | 800 MHz   | 800 MHz   | 800 MHz   |

Here's an example 18 DIMM slot Nehalem configuration:

| CPU (Speed)     | Max Mem   | Bank 1    | Bank 2    |
|                 | Speed     | Populated | Populated |   
|-----------------|-----------|-----------|-----------|
| X5570 (2.93GHz) | 1333 MHz  | 1066 MHz* | 800 MHz   |
| X5560 (2.80GHz) | 1333 MHz  | 1066 MHz* | 800 MHz   |
| X5550 (2.66GHz) | 1333 MHz  | 1066 MHz* | 800 MHz   |
| E5540 (2.53GHz) | 1066 MHz  | 1066 MHz  | 800 MHz   |
| E5530 (2.40GHz) | 1066 MHz  | 1066 MHz  | 800 MHz   |
| E5520 (2.26GHz) | 1066 MHz  | 1066 MHz  | 800 MHz   |
| E5506 (2.13GHz) | 800 MHz   | 800 MHz   | 800 MHz   |
| E5504 (2.00GHz) | 800 MHz   | 800 MHz   | 800 MHz   |
| E5502 (1.66GHz) | 800 MHz   | 800 MHz   | 800 MHz   |

\* According to the HP Quick Spec for the BL460 G6, they are able to keep the speed at 1333 MHz with 2 DIMMS. A BIOS update is required to achieve this. This is HP specific.

**Common Questions:**

**Q:** What kind of performance decrease will I see by lowering the clock speed of my memory? For example using 6x2GB DIMMs (running at 1333 MHz) vs 12 x 1 GB DIMMs (running at 1066 MHz) to save a little money.

**A:** According to the IBM white paper listed above, we have two main areas of performance to worry about, latency and throughput. The latency difference between 1333 MHz and 800 MHz is about 10%. Memory throughput is another story though. The different between 1333 MHz and 1066 MHz is about 9%. The difference from 1066 MHz to 800 MHz is 28%!

**Q:** What kind of performance increase will I see in a balanced (same amount of memory per channel) system?

**A:** Again, according to the IBM paper, you will see a performance increase if the system is balanced. An exact number isnt given.

**Q:** Which is fastest? Single, dual, or quad rank DIMMS?

**A:** According to the IBM White Paper, dual rank outperforms single rank by 7% in Specjbb2005. Quad rank DIMMs decrease the clock speed to 1066 MHz so they are not faster at this time.

**Q:** What if I only populate one processor?

**A:** You want to populate both sockets if performance is a concern. Adding the second processor not only makes the second set of DIMM sockets available, it also doubles the memory bandwidth.

[1]: {{< relref "2009-04-07-introduction-to-nehalem-cpus.md" >}}
