---
author: slowe
title: Learning NVP/NSX
url: /learning-nvp-nsx/
---

For ease of reference, here are links to all of the entries in the "Learning NVP/NSX" blog series. (General note: this content applies to the NVP 3.x/NSX-mh [multi-hypervisor] 4.x release. It does _not_ apply to the NSX-v [NSX for vSphere] 6.x releases.)

[Part 1: High-Level Architecture][1]: This post provides an overview of NVP/NSX and the core components. *(published 5/21/13)*

[Part 2: NVP Controllers][2]: In part 2, you learn how to install and configure a cluster of NVP/NSX controllers. *(published 8/16/13)*

[Part 3: NVP Manager][3]: Part 3 shows you how to install and configure NVP/NSX Manager, a web-based GUI that you use to configure certain aspects of NVP/NSX. *(published 8/19/13)*

[Part 4: Adding Hypervisors to NVP][4]: This post walks through the process for adding hypervisors (the example provided is KVM) to NVP/NSX. *(published 8/22/13)*

[Part 5: Creating a Logical Network][5]: This part shows you how to create a logical network that can be used to connect VMs independently of the underlying physical network topology. *(published 9/6/13)*

[Part 6: Adding an NVP Gateway][6]: To provide L2/L3 connectivity to logical networks in an NVP/NSX environment, you'll need a gateway appliance. This post shows how to set up a gateway appliance and add it to NVP/NSX. *(published 10/28/13)*

[Part 7: Handling the NVP to NSX Transition][7]: This post discusses the planned approach for transitioning from NVP to NSX. *(published 11/1/13)*

[Part 8: An Update on the NVP to NSX Transition][8]: This post provides an overview of the NVP-to-NSX upgrade process, and completes the transition of the series to focus on NSX. *(published 12/5/13)*

[Part 9: Adding a Gateway Service][9]: Part 9 leverages the gateway appliance added in part 6 to create a L3 gateway service that provides routed connectivity in and out of NSX-hosted logical networks. *(published 2/26/14)*

[Part 10: Adding a Service Node][10]: In this part, I show you how to add a service node to your NSX installation. *(published 2/27/14)*

[Part 11: Reviewing OpenStack Integration Basics][11]: This part introduces some concepts related to VMware NSX integration with OpenStack, and provides some basic information on how the integration works. *(published 3/12/14)*

[Part 12: Integrating VMware NSX with OpenStack][12]: Part 12 builds on Part 11 by providing specific details on how to configure OpenStack Neutron to work with VMware NSX. *(published 4/25/14)*

[Part 13: Revisiting Logical Networking:][13] Part 13 revisits the idea of logical switches and logical switch ports in the context of using VMware NSX in an OpenStack environment. *(published 4/28/14)*

[Part 14: Using Logical Routing:][14] Part 14 discusses NSX's logical routing functionality, including distributed logical routing, and discusses logical routing in an OpenStack context. *(published 6/20/14)*

[Part 15: NSX Gateways, Gateway Services, and Logical Routers:][15] Part 15 dives a bit deeper into the core components of NSX's logical routing functionality, and explores the relationships between the different components. *(published 7/16/14)*

[Part 16: Routing to Multiple External VLANs:][16] Part 16 describes how to configure the NSX gateway appliances and OpenStack to support multiple external networks on different VLANs. *(published 10/13/14)*

[Part 17: Adding External L2 Connectivity:][17] Part 17 describes the use of NSX gateway appliances and L2 gateway services with OpenStack to add external L2 connectivity to logical networks. *(published 10/27/14)*

[Part 18: Routing Without Network Address Translation:][18] Part 18 describes the use of logical routers in NSX that do not perform network address translation (NAT). *(published 11/3/14)*

As additional entries are published, I'll update this page accordingly. Enjoy!

[1]: {{< relref "2013-05-21-learning-nvp-part-1-high-level-architecture.md" >}}
[2]: {{< relref "2013-08-16-learning-nvp-part-2-nvp-controllers.md" >}}
[3]: {{< relref "2013-08-19-learning-nvp-part-3-nvp-manager.md" >}}
[4]: {{< relref "2013-08-22-learning-nvp-part-4-adding-hypervisors-to-nvp.md" >}}
[5]: {{< relref "2013-09-06-learning-nvp-part-5-creating-a-logical-network.md" >}}
[6]: {{< relref "2013-10-28-learning-nvp-part-6-adding-an-nvp-gateway.md" >}}
[7]: {{< relref "2013-11-01-learning-nvp-part-7-handling-the-nvp-to-nsx-transition.md" >}}
[8]: {{< relref "2013-12-05-learning-nvp-part-8-an-update-on-the-nvp-to-nsx-transition.md" >}}
[9]: {{< relref "2014-02-26-learning-nsx-part-9-adding-a-gateway-service.md" >}}
[10]: {{< relref "2014-02-27-learning-nsx-part-10-adding-a-service-node.md" >}}
[11]: {{< relref "2014-03-12-learning-nsx-part-11-reviewing-openstack-integration-basics.md" >}}
[12]: {{< relref "2014-04-25-learning-nsx-part-12-integrating-vmware-nsx-with-openstack.md" >}}
[13]: {{< relref "2014-04-28-learning-nsx-part-13-revisiting-logical-networking.md" >}}
[14]: {{< relref "2014-06-20-learning-nsx-part-14-using-logical-routing.md" >}}
[15]: {{< relref "2014-07-16-learning-nsx-part-15-nsx-gateways-gateway-services-and-logical-routers.md" >}}
[16]: {{< relref "2014-10-13-learning-nsx-part-16-routing-to-multiple-external-vlans.md" >}}
[17]: {{< relref "2014-10-27-learning-nsx-part-17-adding-external-l2-connectivity.md" >}}
[18]: {{< relref "2014-11-03-learning-nsx-part-18-routing-without-network-address-translation.md" >}}
