---
layout: single
title: Outbound Trigger
excerpt: "Outbound Trigger"
permalink: /integration-app/outbound-trigger/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---

An **outbound trigger** defines the conditions under which the Integration App should create and send a message to a Service Provider. It monitors activity on a specific table and determines when an outbound message should be generated.

## Fields

- **Name**  
  A meaningful name for easy identification of the trigger.

- **Message Type**  
  The outbound message type that should be sent when the trigger conditions are met.

- **Service Provider**  
  The Service Provider to which this trigger applies.

- **Table**  
  The primary table the trigger will monitor. This selection also determines which fields are available for use in the condition.

- **Extended Tables**  
  If the selected table has child tables (e.g., Configuration Items), you can include them here so the trigger also monitors those tables.

- **Description**  
  A helpful description to clarify the purpose of the trigger for other users—or for your future self.

## Trigger Events

- **Insert**  
  Check this box if the trigger should fire on **record creation**.

- **Update**  
  Check this box if the trigger should fire on **record updates**.

- **Delete**  
  Check this box if the trigger should fire on **record deletion**.

## Trigger Conditions

- **Condition**  
  Define additional filter criteria to control when the trigger should fire.

- **Use Scripted Condition**  
  Enable this to use a script instead of standard conditions.

- **Script Condition**  
  Enter a script that sets the `result` variable to `true` or `false` to determine if the trigger should execute.