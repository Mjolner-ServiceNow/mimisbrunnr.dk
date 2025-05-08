---
layout: single
title: Field Mapping in the Integration App
excerpt: "Field Mapping in the Integration App"
permalink: /integration-app/field-map/
sidebar:
  nav: "ia"
last_modified_at: 
toc: false
---


For both **inbound** and **outbound** messages, **field maps** are essential. They define how fields from the Service Provider correspond to fields in your ServiceNow instance.

## Mapping Options

- **Exclude in mapping**  
  When checked, the Integration App will validate the field but **exclude its output value from the payload**.

- **Unique Identifier**  
  Marks this field as the unique identifier from the Service Provider. Its value will be stored in the **External IDs** table and used to determine whether to create a new record or update an existing one.

- **Always include in payload**  
  Ensures the field is **always included** in the payload, even if its value hasn't changed.

- **Include empty values in payload**  
  Includes the field in the payload **even if its value is empty**.

- **Replace null value with `%empty%`**  
  Replaces `null` values with `%empty%` to accommodate Service Providers that cannot handle `null`.

## Field (ServiceNow Side)

- **Use script**  
  Use a script to dynamically determine the field's value.

- **Field**  
  Select the field from your ServiceNow instance that you want to map.

- **Output Value**  
  Choose whether to use the **database value** or the **display value** in the payload.

## External Field (Service Provider Side)

- **Use script**  
  Use a script to dynamically set the value of the external field.

- **External field name**  
  Specify the name of the field at the Service Provider.

## JSON Options

- **Use JSON datatype**  
  Use actual data types in the payload (e.g., `null` instead of `"null"`, `1` instead of `"1"`).

- **JSON encode output string**  
  Encodes each string value as valid JSON. Cannot be used if **No escape** is selected.

- **No escape**  
  Select this to not escape all string values. This is useful if you want to add objects or more advanced output into the payload. Cannot be used if **JSON encode output string** is selected.