# Azure VM-Series HA cluster fast failover
Azure template and PAN-OS bootstrap package to deploy an Active/Passive HA cluster for VM-Series in Microsoft Azure

## Brief Description
This repository proposes an Azure ARM template that will deploy a pair of VM-Series.
This pair of VM-Series is automatically configured as an active/passive HA cluster with a specific configuration.
An inbound public load balancer and an internal outbound load balancer are deployed to ensure failover mechanism.
A Linux instance with preconfigured Apache web server is deployed and matching PAN-OS security rules and NAT rules are setup in VM-Series.

The goal of this design is to propose a VM-Series high availability mechanism where:
- Failover is fast within a VM-Series HA cluster.
- Session synchronization is enabled.
- Source NAT is not needed for any traffic use case, specifically inbound traffic

## Template Details
- Authoring Group: Public Cloud, Networking, High availability
- Documentation: Azure VM-Series Fast HA.pdf
- Github Location: https://github.com/cestebanez91/Azure-VmSeries-Fast-HA
- PAN-OS Supported: 8.1.0 minimum
- Cloud Provider(s) Supported:  Azure only dedicated



# Diagram and content
![alt text](https://github.com/cestebanez91/Azure-VmSeries-Fast-HA/blob/main/AzureFastHAdiagram.png)


## Content is the following:
-	azureDeploy.json  This file is a **customized version** of Matt McLimans original template available here:  
https://github.com/wwce/azure-arm/tree/master/Azure-Common-Deployments/v1/2fw_3nic_avset_intlb_extlb  
See details for options (licensing mode, new Vnet, AV Set, default credentials, etc..).  
Bootstrapping option will be used to configure VM-Series HA cluster with the below package.

- PAN-OS bootstrap packages for VM1 and VM2 as they have a different initial configuration.  
VM1 and VM2 directories and file are in bootstrap zip archive too.  
Only init-cfg.txt and bootstrap.xml files are needed and used.  
You can use your license authcode is licensing directory.

## Default Username and password
Default username and password for both VM-Series and Linux instance azure
**paloalto/paloalto123!**


# Support Policy
The code and templates in the repo are released under an as-is, best effort, support policy. These scripts should be seen as community supported and Palo Alto Networks will contribute our expertise as and when possible. We do not provide technical support or help in using or troubleshooting the components of the project through our normal support options such as Palo Alto Networks support teams, or ASC (Authorized Support Centers) partners and backline support options. The underlying product used (the VM-Series firewall) by the scripts or templates are still supported, but the support is only for the product functionality and not for help in deploying or using the template or script itself. Unless explicitly tagged, all projects or work posted in our GitHub repository (at https://github.com/PaloAltoNetworks) or sites other than our official Downloads page on https://support.paloaltonetworks.com are provided under the best effort policy.
