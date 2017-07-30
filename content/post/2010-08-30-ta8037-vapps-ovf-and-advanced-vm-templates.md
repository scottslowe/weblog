---
author: slowe
categories: Liveblog
comments: true
date: 2010-08-30T16:32:59Z
slug: ta8037-vapps-ovf-and-advanced-vm-templates
tags:
- Virtualization
- VMware
- VMworld2010
- vSphere
title: 'TA8037: vApps, OVF, and Advanced VM Templates'
url: /2010/08/30/ta8037-vapps-ovf-and-advanced-vm-templates/
wordpress_id: 2055
---

I managed to score a seat in the vApps/OVF/Advanced VM Templates session. Unfortunately, I arrived late, so I don't know the presenter's names (apparently the location of the session was changed from the time I put it on my calendar to today).

The OVF XML descriptor file contains package meta-data and has 10 core sections for describing virtual hardware, EULA, product information, upgrade instructions, etc. The actual software in an OVF is installed in one more more virtual disks, and any public specified virtual disk format is supported. OVF also supports signing, compression, and internationalization.

The presenters showed a quick demonstration of deploying an OVF template using the vSphere Client. (They showed off deploying the SugarCRM vApp.) In particular, they pointed out the product information, version, size, description, etc., stored in the OVF XML meta-data, and mentioned that this can help users avoid downloading the wrong virtual appliance. The presenters also showed deployment options in the OVF XML; this allows the vendor to show recommended configurations for evaluation, production, enterprise, etc.; this is all driven by the vendors and is all stored in the OVF XML package descriptor.

The presenters showed IP address allocation parameters using data stored in the OVF. This functionality simplifies the configuration of the virtual appliance or vApp.

vApps have power commands just like VMs, but they contain multiple VMs. Even though vApps contain multiple VMs, when deploying a vApp via OVF, it doesn't ask you questions about multiple VMs or such. In general, this is handled by the author of the OVF XML package descriptor for the vApp. In the Inventory view, a vApp can be expanded to show the individual VMs contained within the vApp.

Next the presenters discussed creating a vApp from scratch. To create a new vApp, you just right-click on a host and select Create New vApp. Then you just drag existing VMs into/onto the new vApp. Once the new vApp is created, you can populate additional information like product name, product version, VM startup order, timing sequences, and shutdown actions. The presenter showed shutting down a vApp so that we could see how the shutdown order was enforced.

You can also export a vApp as an OVF template. This is a simple command from within the vSphere Client, and it exports the VMDKs and creates the XML descriptor file.

We also saw how to add vApp information to existing VMs without creating a vApp.

The presenters now moved into a discussion of VM templates and how VM templates can be enhanced and extended with vApp properties. There are two primary roles when it comes to templates: the author, who creates it once, validates it, and certifies it, but this occurs rarely. The user, on the other hand, uses these templates frequently to deploy new VMs.

Behind the scenes during a "normal" VM template deployment, it first makes a clone of the existing template. Then it powers it on and installs an agent into the guest OS. The agent is responsible for modifying the guest OS according to the customization specification settings selected during the deployment process. At the end, the new VM is powered off and the deployment is done.

To avoid some of the common limitations of the "normal" way of deploying VM templates, we can incorporate vApp functionality. In the vApp style of deployment, the author is responsible for creating and providing the agent that will customize the guest OS. This might be a shell script or a PowerCLI script. This agent or tool then responds based on parameters passed to it based on information supplied by the user during the deployment process. (Refer back to the description of vApp deployment.) This makes the authoring process harder (but this occurs rarely) and makes the deployment process easier (this occurs more often).

The presenters next moved into a demonstration of using vApp properties and OVF to enhance standard VM template deployment.

The VMware OVF Tool 2.0 is available with Fusion 3.1 and Workstation 7.1 or can be downloaded from [http://www.vmware.com/go/ovf](http://www.vmware.com/go/ovf). OVF Tool can convert OVF to OVA and a variety of other tasks. Another tool is called vAppRun, which integrates with OVFTool and lets you work with vApps and OVF Properties while using Fusion and Workstation. It can be downloaded from [http://labs.vmware.com/flings/vapprun](http://labs.vmware.com/flings/vapprun). The presenters showed a demo of using OVF Tool to deploy OVF templates. They also showed using OVF Tool to deploy from Workstation to vSphere, and finally they demonstrated a more complex deployment like SugarCRM. This showed how to deploy complex vApps from the command line using OVF Tool. (Pretty cool, in my opinion, even if it did include a very long and very complex command line instruction.)

VMware Studio 2.1 is a free application that can help in the creation of virtual appliances/vApps and supports full OVF 1.1 support and integration. It's available from http://www.vmware.com/go/studio.

After this the session wrapped up and went into a question-and-answer session.

**SUMMARY:** I like the continued development of OVF and vApps, but I'm not so sure just how useful the idea of using vApp/OVF technologies for VM template deployment will actually be. The primary roadblock is the fact that the author would have to create the customization agent. Otherwise, OVF Tool looks quite handy and is very likely something I will be exploring in more detail.
