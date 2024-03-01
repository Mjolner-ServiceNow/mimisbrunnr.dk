---
author: LKH
title: "Embracing the Future: Migrating to Extension-Based Hybrid Workers Before the 2024 Deadline"
---

In a pivotal move towards enhancing cloud service efficiency and management, Azure has announced the retirement of the Agent-based User Hybrid Worker on 31 August 2024. This decision underscores the importance of transitioning to the more advanced Extension-based User Hybrid Worker, ensuring your cloud operations are not only up to date but also optimized for future demands. This guide provides a comprehensive roadmap for migrating your existing Agent-based Hybrid Workers to the Extension-based model, ensuring a seamless transition ahead of the retirement deadline.

## Why Migrate?

The shift from Agent-based to Extension-based Hybrid Workers represents Azure's commitment to delivering more robust, efficient, and scalable cloud solutions. The Extension-based model offers improved performance, easier management, and better integration with Azure services, making it an essential upgrade for organizations looking to enhance their cloud infrastructure.

## Step-by-Step Migration Guide

To facilitate a smooth transition to the Extension-based Hybrid Worker, follow these detailed steps:

1. **Access Hybrid Worker Groups:** Log into your Azure portal. Under your 'Automation Account', select 'Hybrid Worker Groups' and navigate to your existing Hybrid Worker group's page.

2. **Initiate the Addition of Hybrid Workers:** Within the Hybrid Worker group page, click on 'Hybrid Workers', then '+ Add' to reach the 'Add machines as Hybrid Worker' section.

3. **Select Your Existing Agent-Based Worker:** Look for your Agent-based (V1) Hybrid Worker in the list. If it's not appearing, ensure the Azure Arc Connected Machine agent is installed on your machine. This step is crucial for enabling the transition to an Extension-based model.

4. **Complete the Addition Process:** After selecting your Hybrid Worker, click 'Add' to include it in the group as an Extension-based Hybrid Worker, thus maintaining your existing workflows without interruption.

5. **Confirm the Migration:** Your Hybrid Worker should now be visible in the Platform column as both Agent-based (V1) and Extension-based (V2). This dual listing verifies the successful migration. 

6. **Update your flows in ServiceNow:** Update your flows in ServiceNow to use the new Hybrid Worker Group. Once you've verified that all your automation is now using the new Hybrid Worker Group, you can safely remove the Agent-based version from your setup.

## Preparing for the 31 August 2024 Deadline

With the retirement of the Agent-based User Hybrid Worker set for August 31st, 2024, it's imperative to begin your migration process well in advance. This ensures that your cloud operations remain uninterrupted and benefits from the latest Azure has to offer.

## Scaling Your Migration

For those managing multiple Agent-based Hybrid Workers, leveraging Azure's automation tools such as Bicep, ARM templates, PowerShell cmdlets, REST API, and Azure CLI can significantly expedite the migration process. These tools are designed to automate and streamline the transition, making it manageable and efficient.

Microsoft are offering free templates, which you can access [here](https://learn.microsoft.com/en-us/azure/automation/migrate-existing-agent-based-hybrid-worker-to-extension-based-workers?tabs=bicep-template%2Cwin-hrw#manage-hybrid-worker-extension-using-bicep--arm-templates-rest-api-azure-cli-and-powershell).

## Conclusion

The upcoming retirement of the Agent-based User Hybrid Worker marks a significant milestone in Azure's evolution. By migrating to the Extension-based User Hybrid Worker, your organization will benefit from enhanced efficiency, scalability, and management capabilities. Follow the outlined steps to ensure a smooth transition, and take advantage of Azure's automation tools for a seamless migration experience. Embrace the future of cloud computing by preparing your infrastructure for the demands of tomorrow, today.