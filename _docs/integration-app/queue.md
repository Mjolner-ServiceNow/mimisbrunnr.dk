---
layout: single
title: Message Queue
excerpt: "Message Queue"
permalink: /integration-app/queue/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

The **Message Queue** provides a central and transparent location for monitoring all messages sent from or received by your system.

## Outbound Messages

For outbound messages using the **Generic Pull** method, this table serves as the source your Service Provider will query to retrieve messages it needs to process.

## Inbound Messages

Inbound messages are also received and stored in this table.

- If the message is to be **processed synchronously**, it will be marked as **Direct Input**.
- If the message is to be **processed asynchronously**, it will be marked as **Input** and handled later by the system.