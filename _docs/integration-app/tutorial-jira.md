---
layout: single
title: Atlassian Jira Integration
permalink: /integration-app/atlassian-jira-integration/
last_modified_at: 2023-02-28T11:36:18-04:00
sidebar:
  nav: "ia"
toc: true
read_time: true
---

In this guide we will show you how you can use Integration App to setup a bi-directional integration with Atlassian Jira. 

We will keep this guide simple and only focus on Incident integration. This guide should however stil provide you with the knowledge you need to be able to easily expand this to task types etc.

## Create a user in Atlassian Jira

When logged in to Jira click to upper right gear icon and select **User Management**.

Under **Directory** select **Service accounts** and click the **Create a service account** button.

Give the service account a name. In this tutorial we will use ***integration-app*** and click **Next**.

Select the **User** role for Jira and click **Create**.

Click **Create credentials** in the upper right corner.

The recommended approach is to use **OAuth 2.0**, so select this and click **Next**.

Give the OAuth credentials a name. We will use ***ServiceNow-demo*** for this tutorial. Click **Next**.

For roles we will select **write:jira-work**. You can do it more fine grained, but this will do for this tutorial. Click on **Next**.

Confirm that everything looks okay and click **Next**.

Make sure to copy the **Client ID** and **Client Secret** and then click on **Done**.

## Setup OAuth Client in ServiceNow
In ServiceNow navigate to **System OAuth** -> **Applocation Registry** and click the **New** button in the upper right corner.

Next select **Connect to a third party OAuth Provider**.

Fill in the following fields:
- Name: **Atlassian Jira**
- Client ID: ***The Client ID you copied in the previous step***
- Client Secret: ***The Client Secret that you coied in the previous step***
- Default Grant type: **Client Credentials**
- Token URL: **https://api.atlassian.com/oauth/token**
- Send Credentials: **In Request Body (Form URL-Encoded)**

## Setup Outbound REST Message
Go to **System Web Services** -> **Outbound** -> **REST Message** and click the **New** button in the upper right corner.

Fill in the following fields, but replace ***automize*** with the name of your Jira instance:
- Name: **Atlassian Jira**
- Accessible from: **All application scopes**
- Endpoint: **https://api.atlassian.com/oauth/token/accessible-resources**
- Authentication type: **OAuth 2.0**
- OAuth profile: **Atlassian Jira default_profile**

Right click in the upper grey bar and select **Save**.

Under Related Links click the **Get OAuth Token** to verify that you can get a token. If you do not see a green bar, verify that you did not mistype anything in one of the previous steps.

Click the **Default GET** under **HTTP Methods** at the bottom of the screen.

Click the **Test** link under **Related Links**.

In the **Response** field you should be able to locate a parameter called **id**. Copy the value after it, which should be a UUID looking something like this: ***09d282e6-fad7-3bcf-92c8-743a2c51a7fb***

This is also referred to as your **Cloud ID**. Make sure to note it down, as we will use it for communicating with Jira going forward.

Click the back button i the upper left corner to get back to the **Rest Message**.

Select the **HTTP Request** section and add the following two

Name: **Accept**
Value **application/json**

Name: **Content-Type**
Value: **application/json**

Right click the grey bar at the top and select **Save**.

In the bottom under **HTTP Methods** click the **New** button to the right.

In the **Name** field write **Create Jira Issue**.

In the **HTTP method** select **POST**.

Set the **Endpoint** to **https://api.atlassian.com/ex/jira/CLOUD_ID/rest/api/3/issue** where you replace ***CLOUD_ID*** with the **Cloud ID** that you noted down earlier.

**Authentication type** should be set to **Inherit from parent**.

Click the **HTTP Request** tab and paste in the follwoing JSON in the **Content** field:

```json
{
  "fields": {
    "project": {
      "key": "PROJ"
    },
    "summary": "Test issue created via API",
    "description": {
      "type": "doc",
      "version": 1,
      "content": [
        {
          "type": "paragraph",
          "content": [
            {
              "type": "text",
              "text": "This is the description of the issue created through the API."
            }
          ]
        }
      ]
    },
    "issuetype": {
      "name": "Task"
    },
    "priority": {
      "name": "Medium"
    }
  }
}
```

Replace **PROJ** with the ID of the Space you wish to add an issue to. You can find the Space ID in the URL if you open the Space in Jira. Look in the URL right after **/project/**.

Warning: In the next step we will create an issue in Jira.

Click the **Test** link under **Related Links** to send a request to Jira.

You should receive a **HTTP status** of **201** and a response containing and ***id***, ***key*** and ***self*** of the newly create issue in Jira.

Go back to the **HTTP Method** and update the **Content** field, so that it uses variables instead of the hardcoded text we used for testing.

```json
{
  "fields": {
    "project": {
      "key": "PROJ"
    },
    "summary": "${short_description}",
    "description": {
      "type": "doc",
      "version": 1,
      "content": [
        {
          "type": "paragraph",
          "content": [
            {
              "type": "text",
              "text": "${description}"
            }
          ]
        }
      ]
    },
    "issuetype": {
      "name": "Task"
    },
    "priority": {
      "name": "${priority}"
    }
  }
}
```

Notice that you can change the **name** attribute of the **issuetype** object to any work type, eg. Incident, that you have defined in your Space in Jira. 

Go back to the **HTTP Message** and click the **New** button under **HTTP Methods** related list at the bottom.

Set the **Name** to **Update Jira Issue**.
The **HTTP method** should be set to **PUT**.
Set the **Endpoint** to **https://api.atlassian.com/ex/jira/CLOUD_ID/rest/api/3/issue/${issueId}**. Remember to replace ***CLOUD_ID*** with your Cloud ID.

Click the **HTTP Request** tab and insert the following in the **Content** field:

```json
{
  "fields": {
    "summary": "${short_description}",
    "description": {
      "type": "doc",
      "version": 1,
      "content": [
        {
          "type": "paragraph",
          "content": [
            {
              "type": "text",
              "text": "${description}"
            }
          ]
        }
      ]
    },
    "priority": {
      "name": "${priority}"
    }
  }
}
```

## Create a user in ServiceNow

The next thing we will do is to create an integration user in ServiceNow. This user should have be created with the following **Internal Integration User** set to **true** and **Identity Type** set to **Machine**.

Set a **password** for the user and note down the **User ID** and **password**. Make sure that **Active** is  set to **true** and that **Password needs reset** is set to **false**.

Next assign the role **x_autps_int_app.integration_user** to the newly created user

### Get Base64 for authentication

Before we move on to setup the trigger in Jira we need the Base64 of the **UserID** followed by a ***colon*** and then the **password**. There are more ways to do this, but one way is to open up a terminal on your computer and type the following:

```echo -n "user_id:password" | base64```

Where ***user_id*** is replace witn **User ID** of the integration user and ***password*** is the ***password*** that you just set for the integration user.

Remove any trailing equal signs (=) from the base64 encoded string that is returned. Notice that Base64 is not an encryption so keep this string secure.

### Create Service Provider in ServiceNow

In ServiceNow go to **Integration App** -> **Service Providers**. Click the **New** button in the upper right corner.

Set name to **Atlassian Jira** and check the **Active** field. Click **Submit**.

Open the new created **Service Provider** and click on **Edit...** under the **Integration Users** tab at the bottom of the screen.

Select the user that you have just created, by dragger the user to the right column using the arrow buttons.

Click on **Save**.

## Create Outbound Message Type

To get ServiceNow to send updates to Jira we will need to configure an Outbound Message Type in Integration App.

In the application navigator in ServiceNow go to **Integration App** -> **Message Types** -> **Outbound** and click the **New** button in the upper right corner.

Set the following fields:
- Name: **Outbound Jira Issue**
- Target Table: **Incident**
- Service Providers: **Atlassian Jira**

In the **Outbound configuration** tab set the following fields:
- Outbound Type: **Direct REST Call**
- Distinct Updated: **Checked**
- Create respone: **Unchecked**
- Payload format: **JSON**
- REST Message: **Atlassian Jira**
- REST HTTP Method: **Create Jira Issue**
- Update REST HTTP Method: **Update Jira Issue**
- Use IntApp Payload as body: **Unchecked**

In the **Result configuration** tab set the following fields:
- Unique ID Path: **id**
- Update correlation_id: **Checked**

Click on **Submit** button to create the Outbound Message Type.

### Configure Field Map

Open the new created Outbound Message Type record and go to the **Field maps** related list at the bottom of the page.

Click the **New** button in the upper right corner of the realted list.

Set the field **Always include in payload** to **Checked**.

In the **Field** tab set the **Field** to **Short description**

In the **External Field** tab set the **External fieldname** to **short_description**.

Click the **Submit** button.

Click the **New** button again.

Set the field **Always include in payload** to **Checked**.

In the **Field** tab set the **Field** to **Description**

In the **External Field** tab set the **External fieldname** to **description**.

Click the **Submit** button.

Click the **New** button again.

Set the field **Always include in payload** to **Checked**.

In the **Field** tab set the **Use script** to **Checked** and paste the following into the **Field Transform Script** field

```javascript
result = (function(source, current, attachment) {
  const priority = current.getValue('priority');
	switch(priority) {
		case '5':
			return 'Lowest';
		case '4':
			return 'Low';
		case '3':
			return 'Medium';
		case '2':
			return 'High';
		default:
			return 'Highest';
	}
})(source, current, attachment);
```

In the **External Field** tab set the **External fieldname** to **priority**.

Click the **Submit** button.

### Configure Outbound Trigger

Still on the Outbound Message Type record select the **Outbound Triggers** related list at the bottom of the page.

Click the **New** button to the right.

Set the following fields:
- Name: **Atlassian Jira Issue**
- Message Type: **Atlassian Jira Issue**
- Service Provider: **Atlassian Jira Issue**
- Ignore Service Provider Updates: **Checked**
- Insert: **Checked**
- Update: **Checked**
- Delete: **Uncheked**

You then need to decide on a condition. For this tutorial we will trigger when an Incident is assigned to the assignment group **Software**. This is done by setting condition to **Assignment Group**, **is**, **Software**.

Click **Submit**.

To verify that everything works as expected pick any open Incident and assign it to the **Software** assignment group. Next go to **Integration App** -> **Data Transfers** -> **Queue** and see if a record was created in here. You should be able to see both what was sent to Jira and what the response was.

Login to Jira to verify that the Task was created there as expected.

## Create an Inbound Message Type in ServiceNow

In the application navigator in ServiceNow select **Integration App** -> **Message Types** -> **Inbound**. Then click the **New** button in the upper right corner.

Fill in the following fields:
- Name: **Inbound Jira Issue**
- Target Table: **Incident**
- Service Providers: **Atlassian Jira**
- Active: **Checked**

In the **Inbound configuration** tab also set these fields:
- Payload Format: **JSON**
- Integration Mode: **Asyncronous**

Right-click on the grey top bar and select **Save**.

Copy the endpoint. You will need this later, when we setup Jira.

### Setup Inbound Field Maps

Click the **New** button in the upper right corner of the **Field maps** realted list on the Inbound Message Type form.

Set the field **Unique Identifier** to **checked**.

In the External Field tab set the **External fieldname** value to **id**.

Click **Submit**.

Click the **New** button again.

In the **Field** tab set the **Field** to **Description**

In the **External Field** tab set the **Use Script** to **checked** and insert the following in the script field:

```javascript
result = (function(source, current) {
    return current.fields.description.toString(); 
})(source, current);
```

Click the **Submit** button.

Click the **New** button again.

In the **Field** tab set the **Field** to **Short description**

In the **External Field** tab set the **Use Script** to **checked** and insert the following in the script field:

```javascript
result = (function(source, current) {
    return current.fields.summary.toString(); 
})(source, current);
```

Click the **Submit** button.

Click the **New** button again.

In the **Field** tab set the **Field** to **State**

In the **External Field** tab set the **Use Script** to **checked** and insert the following in the script field:

```javascript
result = (function(source, current) {
	const status = current.fields.status.name.toString();
  if(status == "To Do"){
		return "1";
	} else if(status == "In Progress") {
		return "2";
	} else if(status == "Done"){
		return "6";
	}
})(source, current);
```

Notice that you may need to change this mapping if you are using different status names in Jira.

Click the **Submit** button.

## Setting up Atlassian Jira

Go to your Atlassian Jira instance and click on the three dots next to the **Space** you would like integrate to. Select **Space settings**.

In the menu to your left click on **Automation**.

Click on the **Create rule** button in the upper right corner and select **Create from scratch**.

### Setting up the Automation trigger

The trigger in Jira is what defines when the flow that we are about to create will trigger. This is similar to the trigger conditions you probably know from ServiceNow. 

For this tutorial we will select **Work item triggers** and pick the **Field value changed** trigger.

Select the fields **Description**, **Summary**, and **Status**.

There are many other options to pick from including adding a **Manual trigger from work item** or the **Multiple work items events** where you can also decide to trigger the flow if **Work item update** etc.

### Filter out events triggered by ServiceNow

Next we will add an if condition by clicking on **Add component** and select **IF: Add a condition**.

Select the **User condition** and set the following values:
- User: **User who triggered the event**
- Check to perform: **is not**
- Criteria: **Select the user you created for the integration in Jira**

### Add the action to send data to ServiceNow

Click on the **Add component** under the if statement and select **THEN: Add an action**

Under **Automation** pick **Send web reqeust**.

In the **Web request URL** insert the **endpoint** that you copied from the **Inbound Message Type** that you created in ServiceNow.

Set **HTTP method** to **POST** and set **Web request body** to **Issue data (Automation format)**.

Under **Headers** you must add a header with the following values:
- Key: **Authorization**
- Value: **Basic** ***your_base64_string***
- Hidden: **checked**

Replace ***your_base64_string*** with the string you copied from the ***Get Base64 for authentication*** step of this tutorial.

Click on **Validate your request configuration** and then click the **Validate** button. Once clicked you should receive response **200 OK**.
