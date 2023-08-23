---
title: 'Install the Azure AD Connect provisioning agent'
description: Learn how to install the Azure AD Connect provisioning agent and how to configure it in the Azure portal.
services: active-directory
author: billmath
manager: amycolannino
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/20/2023
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
---

# Install the Azure AD Connect provisioning agent

This article walks you through the installation process for the Azure Active Directory (Azure AD) Connect provisioning agent and how to initially configure it in the Azure portal.

> [!IMPORTANT]
> The following installation instructions assume that you've met all the [prerequisites](how-to-prerequisites.md).

>[!NOTE]
>This article deals with installing the provisioning agent by using the wizard. For information about installing the Azure AD Connect provisioning agent by using a CLI, see [Install the Azure AD Connect provisioning agent by using a CLI and PowerShell](how-to-install-pshell.md).

For more information and an example, view the following video:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWK5mR]

## Group Managed Service Accounts
A group Managed Service Account (gMSA) is a managed domain account that provides automatic password management, simplified service principal name (SPN) management, and the ability to delegate the management to other administrators. A gMSA also extends this functionality over multiple servers. Azure AD Connect cloud sync supports and recommends the use of a gMSA for running the agent. For more information, see [Group Managed Service Accounts](how-to-prerequisites.md#group-managed-service-accounts).


### Update an existing agent to use the gMSA
To update an existing agent to use the Group Managed Service Account created during installation, upgrade the agent service to the latest version by running *AADConnectProvisioningAgent.msi*. Now run through the installation wizard again and provide the credentials to create the account when you're prompted to do so.

## Install the agent

[!INCLUDE [active-directory-cloud-sync-how-to-install](../../../../includes/active-directory-cloud-sync-how-to-install.md)]

## Verify the agent installation

[!INCLUDE [active-directory-cloud-sync-how-to-verify-installation](../../../../includes/active-directory-cloud-sync-how-to-verify-installation.md)]

>[!IMPORTANT]
> After you've installed the agent, you must configure and enable it before it will start synchronizing users. To configure a new agent, see [Create a new configuration for Azure AD Connect cloud sync](how-to-configure.md).



## Enable password writeback in cloud sync 

You can enable password writeback in SSPR directly in the portal or through PowerShell. 

### Enable password writeback in the portal
To use *password writeback* and enable the self-service password reset (SSPR) service to detect the cloud sync agent, using the portal, complete the following steps: 

 1. Sign in to the [Azure portal](https://portal.azure.com) using a Global Administrator account.
 2. Search for and select **Azure Active Directory**, select **Password reset**, then choose **On-premises integration**.
 3. Check the option for **Enable password write back for synced users** .
 4. (optional) If Azure AD Connect provisioning agents are detected, you can additionally check the option for **Write back passwords with Azure AD Connect cloud sync**.   
 5. Check the option for **Allow users to unlock accounts without resetting their password** to *Yes*.
 6. When ready, select **Save**.

### Using PowerShell

To use *password writeback* and enable the self-service password reset (SSPR) service to detect the cloud sync agent, use the `Set-AADCloudSyncPasswordWritebackConfiguration` cmdlet and the tenant’s global administrator credentials: 

  ```   
   Import-Module "C:\\Program Files\\Microsoft Azure AD Connect Provisioning Agent\\Microsoft.CloudSync.Powershell.dll" 
   Set-AADCloudSyncPasswordWritebackConfiguration -Enable $true -Credential $(Get-Credential)
  ```

For more information about using password writeback with Azure AD Connect cloud sync, see [Tutorial: Enable cloud sync self-service password reset writeback to an on-premises environment (preview)](../../../active-directory/authentication/tutorial-enable-cloud-sync-sspr-writeback.md).

## Install an agent in the US government cloud

By default, the Azure AD Connect provisioning agent is installed in the default Azure environment. If you're installing the agent for US government use, make this change in step 7 of the preceding installation procedure:

- Instead of selecting **Open file**, select **Start** > **Run**, and then go to the *AADConnectProvisioningAgentSetup.exe* file.  In the **Run** box, after the executable, enter **ENVIRONMENTNAME=AzureUSGovernment**, and then select **OK**.

    [![Screenshot that shows how to install an agent in the US government cloud.](media/how-to-install/new-install-12.png)](media/how-to-install/new-install-12.png#lightbox)

## Password hash synchronization and FIPS with cloud sync

If your server has been locked down according to the Federal Information Processing Standard (FIPS), MD5 (message-digest algorithm 5) is disabled.

To enable MD5 for password hash synchronization, do the following:

1. Go to %programfiles%\Microsoft Azure AD Connect Provisioning Agent.
1. Open *AADConnectProvisioningAgent.exe.config*.
1. Go to the configuration/runtime node at the top of the file.
1. Add the `<enforceFIPSPolicy enabled="false"/>` node.
1. Save your changes.

For reference, your code should look like the following snippet:

```xml
<configuration>
   <runtime>
      <enforceFIPSPolicy enabled="false"/>
   </runtime>
</configuration>
```

For information about security and FIPS, see [Azure AD password hash sync, encryption, and FIPS compliance](https://techcommunity.microsoft.com/t5/microsoft-entra-azure-ad-blog/aad-password-sync-encryption-and-fips-compliance/ba-p/243709).

## Next steps 

- [What is provisioning?](../what-is-provisioning.md)
- [What is Azure AD Connect cloud sync?](what-is-cloud-sync.md)
- [Create a new configuration for Azure AD Connect cloud sync](how-to-configure.md).

