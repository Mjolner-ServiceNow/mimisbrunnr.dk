---
layout: single
title: Inbound Message API
excerpt: "Inbound Message API"
permalink: /integration-app/create-message/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

- **Endpoint**: `/api/x_autps_int_app/integration_app/message_type/{message_type_id}/message`  
- **Method**: `POST`

Use this API to send a message to the Integration App for processing.

## Setting the Message Type ID

It is mandatory to include the ID of the Message Type for which you would like to create a message. This is done by entering the **sys_id** of the Inbound Message Type from ServiceNow in the URI. 

Here is an example: `/api/x_autps_int_app/integration_app/message_type/a32601a383bb6210b37693b5eeaad392/message`

The URI for all Inbound Message Types is also visible in the Enpoint field of the Inbound Message Type record in ServiceNow. 

## Optional query parameters

You have the option to include optional query parameters in the URI if you would like.

- **correlation_id**  
  An ID used by the Integration App to look up records in the External IDs table if there is no unique identifier in the field map. It helps determine whether the message should update an existing record or create a new one.

- **external_action**  
  Identifies the action performed by the external party. This is for information purposes only, and may assist you in trouble-shooting scenarios.

- **external_reference**  
  Stores an identifier or number from the third-party system (e.g., ticket ID). This is for information purposes only, and may assist you in trouble-shooting scenarios.

Here is an example of the URI including the optional parameters: `/api/x_autps_int_app/integration_app/message_type/a32601a383bb6210b37693b5eeaad392/message?correlation_id=5d3b0b079721ae10b2ddfbf0f053af2d&external_action=update&external_reference=INC00001001`  

## Payload

The payload can have any format, but must be specified in either XML or JSON. 

If possible, it is recommended to keep the payload as simple as possible and to only contain the fields that are relevant for the integration. This will help performance, but also make it easier for you, when you work with the field mappings.

Here is an example of a simple payload in JSON for an incident

```json
{
  "short_description": "Printer is not working",
  "urgency": 3,
  "impact": 3,
  "number": "INC00001001"
}
```