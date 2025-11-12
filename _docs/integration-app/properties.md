---
layout: single
title: Properties
excerpt: "Properties"
permalink: /integration-app/properties/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

These properties control key behaviors of the Integration App related to processing reliability and debugging.

## Retry Attempts

Defines the number of times the Integration App can retry processing a message. To initiate the retry process, go to the queue record and in the related links (bottom part of the form) click in _Reprocess message_.

- **Default value:** `5` 

## Debug Mode

A global switch to enable or disable debug logging for Integration App transactions.

- **Yes**: Enables debug logging.
- **No**: Disables debug logging _(default)_.