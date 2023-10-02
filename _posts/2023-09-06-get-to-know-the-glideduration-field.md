---
author: LKH
title: "Get to know the GlideDuration field"
---

In ServiceNow you can use a GlideDuration field to store a duration. 

Behind the scene a GlideDuration field is actually a timestamp, similar to a GlideDateTime field, and the the duration is stored as the time elapsed since january 1st 1970 0:00 GMT. So a GlideDuration with a value of 1 day is a timestamp representing january 2nd 0:00 GMT in the database.

The official documentation for GlideDuration is found [here](https://developer.servicenow.com/dev.do#!/reference/api/vancouver/server/no-namespace/c_GlideDurationScopedAPI). It does however have a few black spots, which we will try to cover here.

## Working with the Duration field as a numeric value

Working with a duration as a numeric value is often times the easiest way. With a numeric value is understood the number of miliseconds since january 1st 1970 0:00 GMT.

So to create a GlideDuration object we can do the following to create a duration of 1 day.

```javascript
var duration = new GlideDuration(1 * 24 * 60 * 1000);
```

So the numeric value of 1444000 is the equalivant of 1 day or january 2nd 0:00 GMT in the database.

If you do not want a duration object, but wants to set the value of the field directly, you can use the [setDateNumericValue](https://developer.servicenow.com/dev.do#!/reference/api/vancouver/server/no-namespace/c_GlideElementScopedAPI#SGE-setDateNumericValue_N) function, since we understand, that a GlideDuration field is actually a datetime field, as explained earlier.

```javascript
gr.duration_field.setDateNumericValue(1 * 24 * 60 * 1000);
gr.update();
```

In the above **gr** is a GlideRecord and **duration_field** is the column name of a GlideDuration field of the GlideRecord.


## Comparing two durations

If we continue to work with durations by using their numeric value we can easily compare two durations fields using the [dateNumericValue](https://developer.servicenow.com/dev.do#!/reference/api/vancouver/server/no-namespace/c_GlideElementScopedAPI#SGE-setDateNumericValue_N) function.

As an example, if you want to compare two GlideDuration fields to establish, if one is larger than the other, you can do something similar to the below:

```javascript
if(gr.field_one.dateNumericValue() > gr.field_two.dateNumericValue()){
    // Your logic
}
```

## Adding or substracting durations

Adding or substracting also becomes easier when working with the duration's numeric values.

Here is an example of adding one day to a GlideDuration field

```javascript
var d = gr.duration_field.dateNumericValue() + (1 * 24 * 60 * 1000);
gr.duration_field.setDateNumericValue(d);
gr.update();
```

> Tip: You can also use the dateNumericValue function to get the duration (difference) between two GlideDateTime fields by substracting their values.

## Working with GlideDuration on a client script

On client scripts we cannot use the numeric value if we are working with the g_form object and wants to use the setValue function.

Here we need to adhere to a string format as the input using this format

```
DAYS HOURS:MINUTES:SECONDS
```

So to set a durations field to one day, we would use the following code where **field** is the name of the duration field.

```javascript
g_form.setValue('field', '1 00:00:00');
```

