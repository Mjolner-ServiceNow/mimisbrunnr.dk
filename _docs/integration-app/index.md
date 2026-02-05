---
layout: single
title: Integration App 
excerpt: "Integration App Introduction"
permalink: /integration-app/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

The Integration App enables seamless ticket and configuration data exchange between your ServiceNow instance and your Service Providers - whether they use ServiceNow or another platform. It supports the synchronization of Incidents, Changes, and Problems, making it ideal for managed service environments, multi-vendor setups, or enterprises with multiple ServiceNow instances. Currently it is designed for the following tables:
- Incident [incident]
- Incident Task [incident_task]
- Change Request [change_request]
- Change Task [change_task]
- Problem [problem]
- Problem Task [problem_task]

The app includes a robust, built-in queue for handling all inbound and outbound messages. Messages can be processed synchronously or asynchronously, based on your integration needs. It supports REST and SOAP for outbound communication and even allows Service Providers to pull updates using REST if direct connectivity is not possible.

With its intuitive field mapping capability, you can easily configure how data is sent and received. Outbound triggers automatically detect and send updates, keeping systems in sync without manual intervention. Best of all, the app does not rely on IntegrationHub and requires no additional licenses or transaction fees, making it a cost-effective and scalable solution. 

## Key features 
- Integrates Tasks (such as Incidents, Changes and Problems) and Attachments with Service Providers and external systems
- Built-in queue handles all inbound and outbound messages
- Supports REST, SOAP, and provider pull integrations
- Field mapping and transformation for easy customization
- No IntegrationHub or extra license/transaction costs


In this guide you can find information of how to install and use Integration App for your integrations.