---
layout: single
title: Create Inbound Message API
excerpt: "Create Inbound Message API"
permalink: /integration-app/create-message/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

- **Endpoint**: `/api/x_autps_int_app/integration_app/CreateMessage`  
- **Method**: `POST`

Use this API to send a message to the Integration App for processing.

## Required Fields (in the request body)

- **payload**  
  The content to be processed, structured according to the defined message type.

- **integrationmode**  
  Defines how the message should be handled:
  - `"asynchronous"`: The message is added to the queue for later processing, and a receipt is sent to the Service Provider.
  - `"synchronous"`: The message is processed immediately, and a response is returned to the Service Provider.

- **integrationPartner**  
  The name of the Service Provider sending the message.

- **correlation_id**  
  An ID used by the Integration App to look up records in the External IDs table if there is no unique identifier in the field map. It helps determine whether the message should update an existing record or create a new one.

- **external_action**  
  Identifies the external action to be executed.

- **external_reference**  
  Stores an identifier or number from the third-party system (e.g., ticket ID).

- **message_type**  
  The name of the Inbound Message Type that should be triggered.

## Inbound Create Message Example

Here is a JSON example of an incident that will be processed asynchronous.

```json
{
  "payload": {
    "short_description": "Printer is not working",
    "urgency": 3,
    "impact": 3,
    "number": "INC00001001"
  },
  "integrationmode": "asynchronous",
  "integrationPartner": "Automize",
  "correlation_id": "5d3b0b079721ae10b2ddfbf0f053af2d",
  "external_action": "update",
  "external_reference": "INC00001001",
  "message_type": "Inbound Automize Incident"
}
```

Here is the same example, but processed syncronously and submitted as XML.

```xml
<CreateMessageRequest>
  <payload>
    <short_description>Printer is not working</short_description>
    <urgency>3</urgency>
    <impact>3</impact>
    <number>INC00001001</number>
  </payload>
  <integrationmode>synchronous</integrationmode>
  <integrationPartner>Automize</integrationPartner>
  <correlation_id>5d3b0b079721ae10b2ddfbf0f053af2d</correlation_id>
  <external_action>update</external_action>
  <external_reference>INC00001001</external_reference>
  <message_type>Inbound Automize Incident</message_type>
</CreateMessageRequest>
```