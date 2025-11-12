---
layout: single
title: Installation and Guided Setup
excerpt: "Installation and Guided Setup"
permalink: /integration-app/setup/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

## Configuration & Admin Guide  
  
This section provides a comprehensive walkthrough of the configuration process for administrators and integrators setting up the Integration App in a ServiceNow instance.  
  
### Pre-requisites  
  
- Scoped application must be **installed and updated from the ServiceNow Store**.  
- Scheduled Job **“IntApp Inbound processing” must be active** to process inbound requests and data.  
- Admin role or delegated roles with access to x_autps_int_app.* tables.  
- REST/SOAP endpoints must be reachable (for outbound push).   
  
### Role Setup  
  
| Role | Description |  
|------|-------------|  
| x_autps_int_app.user | Read-only access to messages and logs |  
| x_autps_int_app.integration_user | Required for API usage |  

### Inbound Processing Scheduled Job  
  
- **Job:** Process Integration Queue  
- Ensure that Scheduled Job “IntApp Inbound processing” is **active**.  
- Runs every 10 seconds  
- Picks up to 1000 messages per cycle  
- **State transitions:** ready → processing → processed or error  

## Step-by-Step Configuration  
  
### Step 1: Define Service Provider  
  
- **Path:** Integration App > Service Providers  
- **Click:** "New"  
- **Fill fields:**  
  - **Name:** Unique name of external system  
  - **Active:** Enable/disable  
- **Add Integration Users:**  
  - **Related list:** Select one or more sys_user accounts (must be Identity type: Machine, or Web Service Access Only enabled for older versions)  
  
> _(Each user tied to a provider can call /CreateMessage only for that provider.)_
  
### Step 2: Configure Message Types  
  
#### A. Inbound Message Type  
  
Inbound Message Type will be used in the Integration App API. For that, the Integration Partner/Service Provider must know the endpoint: `api/x_autps_int_app/integration_app/CreateMessage`  
  
- **Path:** Integration App > Message Types > Inbound  
- **Name:** Descriptive name  
- **Target Table:** Record table (e.g. incident)  
- **Payload Format:** JSON or XML  
- **Service Provider:** Lookup from step 1  
- **Create Incident on Error:** If true, failed inbound creates an incident  
  
#### B. Outbound Message Type  
  
- **Path:** Integration App > Message Types > Outbound  
- **Same fields as above**  
- **Add:**  
  - **Outbound Type:** Push (REST/SOAP) or Pull (partner polls SN)  
  - **Distinct Update:** enable if Outbound Request is different for ‘Updates’ then for ‘Inserts’  
  - **Create Response:** the response for the Outbound Request will generate a new Queue entry in Queue table  
  - **REST/SOAP Message:** Reference to existing message  
  - **REST HTTP Method/SOAP Message Function:** Reference to HTTP Method or Message Function  
  
Where request content and endpoints are defined. If integrations with other Integration App ServiceNow environment, go to Integration App > Documentation, and look for chapter Create Inbound Message API.  
  
- **Unique ID Path:** if defined in the response from the Service Provider/Integration Partner, it means the path to the ID of the related External Record (or correlation ID)  
  
> _(**Best Practice**: Create one message type per target table per integration channel.)_
  
### Step 3: Build Field Maps  
  
- **Path:** In the related list of the Inbound/Outbound Message Type  
- **Click:** "New"  
- **Message Type:** Lookup inbound/outbound message type  
- **Internal Field:** Field in SN record  
- **External Field:** Field in JSON/XML payload  
- **Flags:**  
  - **Is Unique:** For coalescing/upsert logic  
  - **Always Include:** Even if value is null  
  - **Exclude:** Skip this field during transformation  
  - **Replace Null With:** For defaulting values like "%empty%"  
  
Scripting supported for custom transformations and/or validations  
  
> _(Use the Script field to transform values like converting formats or validation of Sys IDs.)_
  
### Step 4: Define Outbound Triggers  
  
- **Path:** In the related list of the Outbound Message Type  
- **Name:** Define Trigger Name  
- **Table:** Select a target table (the same as the Message Type)  
- **Event:** Insert/Update/Delete  
- **Message Type:** Choose outbound type from step 2  
- **Condition:** Glide filter builder  
- **Script Condition:** For more dynamic filtering and/or validation (must return true/false)  
  
> _(Every matching event will enqueue a message to the Queue table. For a table different than [task], please create a Business Rule that triggers the integration. Use Business Rule “IntApp Process Outbound Triggers” as reference)_
  
### Step 5: API Key or OAuth Configuration  
  
For secure API usage:  
  
- Create an API integration user (or use the one in the Service Provider)  
- Grant the user x_autps_int_app.integration_user role (if not granted yet)  
- Store credentials in an API Basic Auth profile or OAuth profile  
- Link it to REST Message if push method is used  
  
> _(If it is an automated message (REST or SOAP), the Integration App does NOT work with no authentication. Please ensure that there’s an authentication method configured.)_
 