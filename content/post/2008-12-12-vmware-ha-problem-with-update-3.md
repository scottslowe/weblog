---
author: slowe
categories: News
comments: true
date: 2008-12-12T17:54:15Z
slug: vmware-ha-problem-with-update-3
tags:
- ESX
- ESXi
- Virtualization
- VMware
- VMwareHA
title: VMware HA Problem with Update 3
url: /2008/12/12/vmware-ha-problem-with-update-3/
wordpress_id: 1094
---

A problem has been identified with VMware ESX 3.5 Update 3 when using VMware HA and VM failure monitoring. This problem results from a delay in the transmission of a heartbeat from a VM to VMware HA; VMware HA then detects this as a VM failure and restarts the VM. It appears that this problem affects both VMware ESX and VMware ESXi.

More information on the problem is available in [this KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1007899).

To fix the problem, users have two options:

1. Disable virtual machine failure monitoring within the VMware HA cluster.

2. Reconfigure the host to change the heartbeat delay.

To reconfigure the host to change the heartbeat delay, follow the steps below:

1. Disconnect the host from VC (right-click on the host in the VI Client and select "Disconnect").

2. Login to the VMware ESX server via SSH and obtain root permissions. Remember that best practices specify not to allow root SSH login, so you'll need to login as an ordinary user and then use `su -` to become root.

3. Using a text editor such as nano or vi, edit the file `/etc/vmware/hostd/config.xml` and set the value of heartbeatDelayInSecs to 0, like this:
    
    ``` xml
    <vmsvc>  
        <heartbeatDelayInSecs>0</heartbeatDelayInSecs>  
    </vmsvc>
    ```

4. Restart the management agents on the VMware ESX server.

5. Reconnect the host in VC (right-click on the host in the VI Client and select "Connect").

No information is yet available on when this issue will be fixed.
