---
layout: single
title: Pull Strategy for Service Providers
excerpt: "Pull Strategy for Service Providers"
permalink: /integration-app/pull-message/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---


If a Service Provider cannot be contacted directly, the Integration App supports a **pull-based strategy** using REST APIs. This allows the Service Provider to fetch and acknowledge messages on demand.

---

## GetReadyMessages

Retrieves all messages that are **ready to be processed** by the specified Service Provider.

- **Endpoint**:  
  `/api/x_autps_int_app/integration_app/GetReadyMessages/{integrationPartner}`

- **Method**: `GET`

- **Path Parameter**:  
  `{integrationPartner}` — Replace with the name of the Service Provider.

- **Query Parameter** *(optional)*:  
  `?limit=100` — Limits the number of messages returned.

This endpoint returns a list of all messages that are ready for the Service Provider to process.

---

## SetProcessed

Marks a message as **processed** once the Service Provider has successfully handled it.

- **Endpoint**:  
  `/api/x_autps_int_app/integration_app/SetProcessed/{id}`

- **Method**: `PATCH`

- **Path Parameter**:  
  `{id}` — The `correlation_id` of the message that has been processed.

The Service Provider should call this endpoint for **each message** it processes. This ensures:
- Messages are not processed more than once.
- Integration App has a clear overview of processed vs. pending messages.

---

> 💡 This pull-based approach is ideal for environments with strict firewall policies or scenarios where push-based communication is not feasible.