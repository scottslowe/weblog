---
author: slowe
categories: Tutorial
comments: true
date: 2010-04-12T09:53:22Z
slug: changing-ip-addresses-on-an-emc-celerra-ns-960
tags:
- Celerra
- CLARiiON
- EMC
- Networking
- Storage
title: Changing IP Addresses on an EMC Celerra NS-960
url: /2010/04/12/changing-ip-addresses-on-an-emc-celerra-ns-960/
wordpress_id: 1876
---

Changing the IP addresses on an EMC Celerra unified storage array is generally a pretty straightforward process. EMC supplies a script called `clariion_mgmt` that you can use to change the IP addresses on the back-end CLARiiON storage processors (SPs). In the large majority of cases, the process for changing the IP addresses on an EMC Celerra using this script looks something like this:

1. Change the IP address of the Control Station using Celerra Manager.

2. Use the `clariion_mgmt` script to update the IP addresses on the back-end CLARiiON SPs.

As with all things in the IT field, this straightforward process sometimes becomes a bit more convoluted. If you are attempting to change the IP addresses on an EMC Celerra using the `clariion_mgmt` script and having some problems, I hope that the information provided in this post is helpful to you. I gathered this information during an instance in which I ran into a series of problems changing the IP addresses on a partner's EMC NS-960 after they re-addressed their lab.

The first step is to change the IP address of the Control Station. This is readily accomplished using Celerra Manager, but remember that you will need to authenticate as "root" and not as "nasadmin". Otherwise, you won't have the right to modify the IP address. Also keep in mind that because the Control Station is separate from the network interfaces on the data movers, changing the IP address on the Control Station won't affect network connectivity to/from the data movers.

Once the Control Station's IP address has been changed, the next step would be to use the `clariion_mgmt` script. The general use of the `clariion_mgmt` script involves the use of the "modify" parameter to change the IP addresses on the back-end CLARiiON SPs. Instead of using the "modify" parameter, the only [blog post](http://whughgriffin.wordpress.com/2009/07/15/how-to-modify-the-sp-ip-addresses-for-an-emc-celerra-ns-proxy-arp-implementation/) I found on this process recommended the use of the "stop" parameter, like this:

	/nasmcd/sbin/clariion_mgmt -stop

This command resets the IP addresses on the CLARiiON SPs back to their default IP addresses (out of the 128.221.x.x range). From there, the next step is supposed to be using the `clariion_mgmt` script again like this:

	/nasmcd/sbin/clariion_mgmt -start -spa_ip _A.B.C.D_ -spb_ip _W.X.Y.Z_ -use_proxy_arp

If this command works---and it usually does---you're good to go. Of course, it didn't work in my case. This is where things start to get interesting. Assuming that the `clariion_mgmt -stop` command worked, you now have two SPs that are not reachable from the network. In order to change their IP addresses, you need to connect to them over the network. Obviously, you can see the problem. EMC provides two answers to this potential problem:

1. You can use the EMC-supplied serial cable to establish a PPP over serial connection to the SP. Devang (aka [@StorageNerve](http://twitter.com/storagenerve)) has an article that provides more details on the [settings for dialup PPP into a CLARiiON](http://storagenerve.com/2008/08/15/clariion-cx-cx3-cx4-settings-for-dailup-ppp-into-the-clariion-machines/). Once you've connected via PPP over serial, you can access the SP using `http://192.168.1.1/setup` in a web browser. This will take you to a screen where you can set the SP's IP address.

2. Every CLARiiON SP also has a LAN service port to which you can connect using a standard straight-through Ethernet cable. The default IP addresses for the LAN service ports are 128.221.1.250 for SP A and 128.221.1.251 for SP B. (These are documented on EMC PowerLink; see EMC Knowledgebase article emc199379.) Once you've connected using the LAN service port, then you can access the SP using `http://<default LAN service port address>/setup` to change the SP's IP address. (Changing the IP address of the SP does not affect the IP address of the LAN service port, so your own network connectivity is not affected.)

In this particular case, I was able to successfully connect to SP A using PPP over serial and change the IP address. For some reason, SP B would not establish a PPP over serial connection, so I had to use the LAN service port. It took only a couple of minutes to change the IP address on my laptop so that I could connect via the LAN service port and change SP B's IP address as well.

Because the `clariion_mgmt` script didn't work, I now needed to update some configuration files on the Celerra so that it would know how to communicate with the back-end CLARiiON SPs. (The `clariion_mgmt` script does this for you.) The specific information on which files need to be modified and the changes that need to be made are found in EMC Knowledgebase articles emc196029. I found that by following all the steps _except step 6_ I was able to get the Celerra talking to the back-end CLARiiON SPs. I verified this by using the `clariion_mgmt` script again as follows:

	/nasmcd/sbin/clariion_mgmt -info

The script returned a correct proxy ARP configuration. In addition, the SPs were pingable both from the Celerra Control Station as well as across the network, and Navisphere worked as expected when accessing the SPs directly via a web browser.

The final step was to verify that the Celerra was properly configured for the updated back-end CLARiiON SPs. To do that, I used information found in EMC Knowledgebase article emc220238; specifically, I used the `nas_storage` command to rediscover back-end storage and ensure that no errors were reported. (By the way, emc220238 also contains information on which files on the Celerra Control Station need to be modified when the back-end SPs have their addresses changed.)

Remember, in the majority of cases the `clariion_mgmt` script will work just fine and it is the preferred method of changing the IP addresses on an integrated Celerra like the NS-960. These other steps are only necessary when the `clariion_mgmt` script does not work, or when you find yourself needing to recover a botched IP address change.

Additional information from those familiar with these technologies and processes is always welcome!
