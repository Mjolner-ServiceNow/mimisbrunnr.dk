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
  An ID used by the Integration App to look up records in the External IDs table. It helps determine whether the message should update an existing record or create a new one.

- **external_action**  
  Identifies the external action to be executed.

- **external_reference**  
  Stores an identifier or number from the third-party system (e.g., ticket ID).

- **message_type**  
  The name of the Inbound Message Type that should be triggered.