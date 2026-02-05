---
layout: single
title: Inbound Message Type
excerpt: "Inbound Message Type"
permalink: /integration-app/inbound-message/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

An **inbound message type** defines the structure of messages your system expects to receive from a Service Provider. To configure an inbound message type, complete the following fields:

## Fields

- **Name**  
  Enter a meaningful name that clearly describes the purpose of the message.
  > _Note: This name will be shared with the Service Provider, so choose something descriptive and recognizable._

- **Service Providers**  
  Select one or more Service Providers that are allowed to send this message type.

- **Target Table**  
  Choose the target table in your ServiceNow instance where the message data will be stored or processed.
  ***Roles required:*** _admin, x_autps_int_app.user_

- **Active**  
  Specify whether the message type should be active.

- **Description**  
  Provide a clear description to help future users (or yourself) understand the purpose and function of this message type.

- **Endpoint**  
  Automatically created the correct endpoint for the Inbound Message Type upon creation.

- **Payload Format**  
  Select the expected format of the incoming payload: **JSON** or **XML**.

- **Integration Mode**  
    Select wether the inbound message should be processed **syncronously** or  **asychronously**.

- **Run Script**  
  Check true if need to run a script after processing the message (onAfter action).

- **Post Processing Script**  
  Script to be executed after processing the message.

## Monitoring

- **Create Incident on Error**  
  Enable this option if you want the Integration App to create an Incident automatically when an error occurs during message processing.

- **Incident Template**  
  If the above option is enabled, specify the **Incident Template** to be used when creating the Incident.

## Field Mapping

After creating your inbound message type, configure the relevant [field maps](/integration-app/field-map) to define how incoming data should be mapped to fields in your target table.