---
title: Monitor Surface Hubs with Azure Monitor | Microsoft Docs
description: Use the Surface Hub solution to track the health of your Surface Hubs and understand how they are being used.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/16/2018
ms.reviewer: shijain

---

# Monitor Surface Hubs with Azure Monitor to track their health

![Surface Hub symbol](./media/surface-hubs/surface-hub-symbol.png)

This article describes how you can use the Surface Hub solution in Azure Monitor to monitor Microsoft Surface Hub devices. The solution helps you track the health of your Surface Hubs as well as understand how they are being used.

Each Surface Hub has the Microsoft Monitoring Agent installed. Its through the agent that you can send data from your Surface Hub to a Log Analytics workspace in Azure Monitor. Log files are read from your Surface Hubs and are then sent to Azure Monitor. Issues like devices being offline, the calendar not syncing, or if the device account is unable to log into Skype are shown in the Surface Hub dashboard in Azure Monitor. By using the data in the dashboard, you can identify devices that are not running, or that are having other problems, and potentially apply fixes for the detected issues.

## Install and configure the solution
Use the following information to install and configure the solution. In order to manage your Surface Hubs in Azure Monitor, you'll need the following:

* A [Log Analytics subscription](https://azure.microsoft.com/pricing/details/log-analytics/) level that will support the number of devices you want to monitor. Log Analytics pricing varies depending on how many devices are enrolled, and how much data it processes. You'll want to take this into consideration when planning your Surface Hub rollout.

The Surface Hub solution is offered as an Azure Marketplace application which is linked to a new or existing Log Analytics workspace within your Azure subscription. Detailed instructions for using either method is at [Create a Log Analytics workspace in the Azure portal](../logs/quick-create-workspace.md). 

To configure the Surface Hub solution, follow these steps:

1. Go to the [Surface Hub page in the Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SurfaceHubOMS?tab=Overview). You might need to login to your Azure subscription to access this.
2. Select **Get it now**.
3. Choose an existing or configure a new Log Analytics Workspace.
4. After your workspace is configured and selected, select **Create**. You'll receive a notification when the solution has been successfully created.

Once the Log Analytics workspace is configured and the solution created, there are two ways to enroll your Surface Hub devices:

* Automatically through Intune
* Manually through **Settings** on your Surface Hub device.

## Set up monitoring
You can monitor the health and activity of your Surface Hub using Azure Monitor. You can enroll the Surface Hub by using Intune, or locally by using **Settings** on the Surface Hub.

## Connect Surface Hubs to Azure Monitor through Intune
You'll need the workspace ID and workspace key for the Log Analytics workspace that will manage your Surface Hubs. You can get those from the workspace settings in the Azure portal.

Intune is a Microsoft product that allows you to centrally manage the Log Analytics workspace configuration settings that are applied to one or more of your devices. Follow these steps to configure your devices through Intune:

1. Sign in to [Microsoft Intune admin center](https://endpoint.microsoft.com/).
2. Go to **Devices** > **Configuration profiles**.
3. Create a new Windows 10 profile, and then select **templates**.
4. In the list of templates, select **Device restrictions (Windows 10 Team)**.
5. Enter a name and description for the profile.
6. For **Azure Operational Insights**, select **Enable**.
7. Enter the Log Analytics **Workspace ID** and enter the **Workspace Key** for the policy.
8. Assign the policy to your group of Surface Hub devices and save the policy.

    :::image type="content" source="./media/surface-hubs/intune.png" alt-text="Screenshot that shows setting an Intune policy.":::

Intune then syncs the Log Analytics settings with the devices in the target group, enrolling them in your Log Analytics workspace.

## Connect Surface Hubs to Azure Monitor using the Settings app
You'll need the workspace ID and workspace key for the Log Analytics workspace that will manage your Surface Hubs. You can get those from the settings for the Log Analytics workspace in the Azure portal.

If you don't use Intune to manage your environment, you can enroll devices manually through **Settings** on each Surface Hub:

1. From your Surface Hub, open **Settings**.
2. Enter the device admin credentials when prompted.
3. Click **This device**, and the under **Monitoring**, click **Configure Log Analytics Settings**.
4. Select **Enable monitoring**.
5. In the Log Analytics settings dialog, type the Log Analytics **Workspace ID** and type the **Workspace Key**. 

    ![Screenshot shows the Microsoft Operations Manager Suite settings with Enable monitoring selected and text boxes for Workspace ID and Workspace Key.](./media/surface-hubs/settings.png)
1. Click **OK** to complete the configuration.

A confirmation appears telling you whether or not the configuration was successfully applied to the device. If it was, a message appears stating that the agent successfully connected to Azure Monitor. The device then starts sending data to Azure Monitor where you can view and act on it.

## Monitor Surface Hubs
Monitoring your Surface Hubs using Azure Monitor is much like monitoring any other enrolled devices.

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

When you click on the Surface Hub tile, your device's health is displayed.

   ![Surface Hub dashboard](./media/surface-hubs/surface-hub-dashboard.png)

You can create [alerts](../alerts/alerts-overview.md) based on existing or custom log searches. Using the data Azure Monitor collects from your Surface Hubs, you can search for issues and alert on the conditions that you define for your devices.

## Next steps
* Use [Log queries in Azure Monitor](../logs/log-query-overview.md) to view detailed Surface Hub data.
* Create [alerts](../alerts/alerts-overview.md) to notify you when issues occur with your Surface Hubs.
