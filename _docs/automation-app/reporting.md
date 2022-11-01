---
layout: single
title: ServiceNow Reporting
permalink: /automation-app/reporting/
last_modified_at: 2022-10-13T08:16:18+01:00
sidebar:
  nav: "aa"
toc: true
---

Reporting on your automation success is key to keeping the momentum on your automation journey and to highligt the value that has been created.

## Dashboard

To access the build in dashboard open the **Automation App** in the Navigator of ServiceNow

![Click on Dashboard](/assets/images/x_autps_azure_auto_menu_dashboard.webp)

Click on **Dashboard** to open the dashboard.

![Dashboard](/assets/images/x_autps_azure_auto_dashboard.webp)

The dashboard is divided into three different main sections.

### Automation Succcess Rate

This part of the dash highlights your automation success rate as measure by number of successful jobs versus the number of jobs that has failed.

If you click on the red part of the graph you can go directly to see which jobs has failed.

### Manual Effort Saved

Manual effort is the time that it would have taken a person to carry out the given task manually.

The pie chart to the right shows the most time saving jobs.

### Eliminated Lead Time

Eliminated lead time is the time that the requester does no longer have to wait before the request is fulfilled.

In other words it is the manual effort that it would have taken to carry out the task manually plus the time that the task would sit in queue and wait for someone to pick it up.

## Data source

The data for the dashboard is based on records in the **jobs** table. It is thus key that the fields **Average manual time to complete** and **Average manual lead time to complete** are filled with the correct estimates when the job is created as part of your [Flow](/automation-app/flow-designer/) or [Workflow](/automation-app/workflow/).
