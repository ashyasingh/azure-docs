---
title: License provisioning guidelines for Extended Security Updates for Windows Server 2012
description: Learn about license provisioning guidelines for Extended Security Updates for Windows Server 2012 through Azure Arc.
ms.date: 08/18/2023
ms.topic: conceptual
---

# License provisioning guidelines for Extended Security Updates for Windows Server 2012

Flexibility is critical when enrolling end of support infrastructure in Extended Security Updates (ESUs) through Azure Arc to receive critical patches. To give ease of options across virtualization and disaster recovery scenarios, you must first provision Windows Server 2012 Arc ESU licenses and then link those licenses to your Azure Arc-enabled servers. The linking and provisioning of licenses can be done through Azure portal, ARM templates, CLI, or Azure Policy.

When provisioning WS2012 ESU licenses, you need to specify whether you'll need to select between virtual core and physical core licensing, select between standard and datacenter licensing, and attest to the number of associated cores (broken down by the number of 2-core and 16-core packs). To assist with this license provisioning process, this article provides general guidance and sample customer scenarios for planning your deployment of WS2012 ESUs through Azure Arc.

## General guidance: Standard vs. Datacenter, Physical vs. Virtual Cores 

### Physical core licensing

If you choose to license based on physical cores, the licensing requires a minimum of 16 physical cores per license. Most customers choose to license based on physical cores and select Standard or Datacenter edition to match their original Windows Server licensing. While Standard licensing can be applied to up to two virtual machines (VMs), Datacenter licensing has no limit to the number of VMs it can be applied to. Depending on the number of VMs covered, it may make sense to opt for the Datacenter license instead of the Standard license.

### Virtual core licensing

If you choose to license based on virtual cores, the licensing requires a minimum of eight virtual cores per Virtual Machine. There are two main scenarios where this model is advisable: 

1. If the VM is running on a third-party host or hyper scaler like AWS, GCP, or OCI.

1. The Windows Server was licensed on a virtualization basis. In most cases, customers elect the Standard edition for virtual core-based licenses.

An additional scenario (scenario 1, below) is a candidate for VM/Virtual core licensing when the WS2012 VMs are running on a newer Windows Server host (that is, Windows Server 2016 or later).

> [!IMPORTANT]
> In all cases, customers are required to attest to their conformance with SA or SPLA. There is no exception for these requirements. Software Assurance or an equivalent Server Subscription is required for customers to purchase Extended Security Updates on-premises and in hosted environments. Customers will be able to purchase Extended Security Updates via Enterprise Agreement (EA), Enterprise Subscription Agreement (EAS), a Server & Cloud Enrollment (SCE), and Enrollment for Education Solutions (EES). On Azure, customers do not need Software Assurance to get free Extended Security Updates, but Software Assurance or Server Subscription is required to take advantage of the Azure Hybrid Benefit.
> 

## Scenario based examples: Compliant and Cost Effective Licensing 

### Scenario 1: Eight modern 32-core hosts (not Windows Server 2012). While each of these hosts are running four 8-core VMs, only one VM on each host is running Windows Server 2012 R2

In this scenario, you can use virtual core-based licensing to avoid covering the entire host by provisioning eight Windows Server 2012 Standard licenses for eight virtual cores each and link each of those licenses to the VMs running Windows Server 2012 R2. Alternatively, you could consider consolidating your Windows Server 2012 R2 VMs into two of the hosts to take advantage of physical core-based licensing options. 

### Scenario 2: A branch office with four VMs, each 8-cores, on a 32-core Windows Server 2012 Standard host

In this case, you should provision two WS2012 Standard licenses for 16 physical cores each and apply to the four Arc-enabled servers. Alternatively, you could provision four WS2012 Standard licenses for eight virtual cores each and apply individually to the four Arc-enabled servers. 

### Scenario 3: Eight physical servers in retail stores, each server is standard with eight cores each and there's no virtualization 

In this scenario, you should apply eight WS2012 Standard licenses for 16 physical cores each and link each license to a physical server. Note that the 16 physical core minimum applies to the provisioned licenses. 

### Scenario 4: Multicloud environment with 12 AWS VMs, each of which have 12 cores and are running Windows Server 2012 R2 Standard

In this scenario, you should apply 12 Windows Server 2012 Standard licenses with 12 virtual cores each, and link individually to each AWS VM.

### Scenario 5: Customer has already purchased the traditional Windows Server 2012 ESUs through Volume Licensing

In this scenario, the Azure Arc-enabled servers that have been enrolled in Extended Security Updates through an activated MAK Key are as enrolled in ESUs in Azure portal. You have the flexibility to switch from this key-based traditional ESU model to WS2012 ESUs enabled by Azure Arc between Year 1 and Year 2. 

### Scenario 6: Migrating or retiring your Azure Arc-enabled servers enrolled in Windows Server 2012 ESUs

In this scenario, you can deactivate or decommission the ESU Licenses associated with these servers. If only part of the server estate covered by a license no longer requires ESUs, you can modify the ESU license details to reduce the number of associated cores.  

### Scenario 7: 128-core Windows Server 2012 Datacenter server running between 10 and 15 Windows Server 2012 R2 VMs that get provisioned and deprovisioned regularly
 
In this scenario, you should provision a Windows Server 2012 Datacenter license associated with 128 physical cores and link this license to the Arc-enabled Windows Server 2012 R2 VMs running on it. The deletion of the underlying VM also deletes the corresponding Arc-enabled server resource, enabling you to link another Arc-enabled server. 

## Next steps

* Find out more about [planning for Windows Server and SQL Server end of support](https://www.microsoft.com/en-us/windows-server/extended-security-updates) and [getting Extended Security Updates](/windows-server/get-started/extended-security-updates-deploy).

* Learn about best practices and design patterns through the [Azure Arc landing zone accelerator for hybrid and multicloud](/azure/cloud-adoption-framework/scenarios/hybrid/arc-enabled-servers/eslz-identity-and-access-management).
* Learn more about [Arc-enabled servers](overview.md) and how they work with Azure through the Azure Connected Machine agent.
* Explore options for [onboarding your machines](plan-at-scale-deployment.md) to Azure Arc-enabled servers.