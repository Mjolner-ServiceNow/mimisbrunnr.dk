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

A global switch to enable or disable debug logging for Integration App transactions. Is set to **private**.

- **Yes**: Enables debug logging.
- **No**: Disables debug logging _(default)_.

## Tables

Comma-separated list of tables that can be used with Integration App. Is set to **private** and only editable by **admin**.