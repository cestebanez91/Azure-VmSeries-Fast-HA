# Azure VM-Series HA cluster fast failover
Azure template and PAN-OS bootstrap package to deploy an Active/Passive HA cluster for VM-Series in Microsoft Azure with fast failover.

## Brief Description
This repository proposes an Azure ARM template that will deploy a pair of VM-Series.  
This pair of VM-Series is automatically configured as an active/passive HA cluster with a specific configuration.  
An inbound public load balancer and an internal outbound load balancer are deployed to ensure failover mechanism.  
A Linux instance with preconfigured Apache web server is deployed and matching PAN-OS security rules and NAT rules are setup in VM-Series.

The goal of this design is to propose a VM-Series high availability mechanism where:
- Failover is fast within a VM-Series HA cluster.
- Session synchronization is enabled.
- Source NAT is not needed for any traffic use case, specifically inbound traffic.

## Template Details
- Authoring Group: Public Cloud, Networking, High availability
- Documentation: Azure VM-Series Fast HA.pdf
- Github Location: https://github.com/cestebanez91/Azure-VmSeries-Fast-HA
- PAN-OS Supported: 8.1.0 minimum
- Cloud Provider(s) Supported:  Azure only dedicated



# Diagram and content
![alt text](https://github.com/cestebanez91/Azure-VmSeries-Fast-HA/blob/main/AzureFastHAdiagram.png)


## Content is the following:
-	azureDeploy.json  
This file is a **customized version** of Matt McLimans original template available here:  
https://github.com/wwce/azure-arm/tree/master/Azure-Common-Deployments/v1/2fw_3nic_avset_intlb_extlb  
See details for options (licensing mode, new Vnet, AV Set, default credentials, etc..).  
Bootstrapping option will be used to configure VM-Series HA cluster with the below package.

- PAN-OS bootstrap packages for VM1 and VM2 as they have a different initial configuration.  
VM1 and VM2 directories and file are in bootstrap zip archive too.  
Only init-cfg.txt and bootstrap.xml files are needed and used.  
You can use your license authcode in licensing directory.

## Default Username and password
Default username and password for both VM-Series and Linux instance are **paloalto/paloalto123!**

## Session synchronization behavior
VM-Series are configured as a HA cluster, session synchronization is enabled.  
However, when primary VM-Series is lost and secondary VM-series becomes active, existing session are active on the new node but traffic is not passing through.  
This is due to the basic behavior of Azure Load balancer which is not balancing traffic per packet but per session.  
Once a session is initiated and use one of the VM-Series, the session is still sent to this VM-Series, whatever the health probe status.  
So session synchronization is not really usefull, except if your application is able to reinitiate the session with the corresponding session id.  
When primary node is back to active state (and the session did not reach the Load balancer time out), packets of an existing session can pass the first VM-Series (with a ssh session for example) because session is synchronized.  
By the way, Preemtive flag is not enabled in the HA cluster configuration by default.


# Deployment
All default value should be ok.
Default IP parameters used for Azure Vnet, subnets and network interfaces are used by the bootstrap.xml configuration file.
If you change it, you PAN-OS configuration has to be modified as well.  

[<img src="http://azuredeploy.net/deploybutton.png"/>](https://portal.azure.com/#create/Microsoft.Template/uri/https://github.com/cestebanez91/Azure-VmSeries-Fast-HA/blob/main/azureDeploy.json)


# Support Policy
The code and templates in the repo are released under an as-is, best effort, support policy. These scripts should be seen as community supported and Palo Alto Networks will contribute our expertise as and when possible. We do not provide technical support or help in using or troubleshooting the components of the project through our normal support options such as Palo Alto Networks support teams, or ASC (Authorized Support Centers) partners and backline support options. The underlying product used (the VM-Series firewall) by the scripts or templates are still supported, but the support is only for the product functionality and not for help in deploying or using the template or script itself. Unless explicitly tagged, all projects or work posted in our GitHub repository (at https://github.com/PaloAltoNetworks) or sites other than our official Downloads page on https://support.paloaltonetworks.com are provided under the best effort policy.
