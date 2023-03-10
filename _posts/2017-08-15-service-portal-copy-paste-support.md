---
author: LKH
title: "How to support copy / paste of pictures on Service Portal"
---

The support for users to easily copy / paste images, often times screenshots, to Service Portal is a requirement often seen. Here is how I have accomplished this.

You need to apply this change to any widget that you want to support copy/paste. In this example we will use the "Ticket Conversations" widget, but the same logic can be applied to "SC Catalog Item" or "Ticket Attachments" widget.

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support0.webp)

Open your instance and go to /sp_config. The click the Widget Editor and open "Ticket Conversations".

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support1.webp)

We cannot edit this widget as it is read only. Thus you need to create a copy of it by clicking the burger icon to the right and select "Clone "Ticket Conversations""

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support2.webp)

Giv your new copy a meaningfull name.

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support3.webp)

Place a checkmark in "HTML Template" and "Client Script". These are the only places we will make changes.

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support4.webp)

On line 6 add the attribute   ng-paste="paste($event)" inside the <div> tag on the HTML part to the left.

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support5.webp)

In the "Client Script" window to your right add the $scope.paste function. It is not so important where you add it, just make sure not to place at the same level as the other functions. I added mine on line 56. Here is the code for copy/paste:

```javascript
$scope.paste = function (event) {
  var files = [];
  var clipData = event.originalEvent.clipboardData;
  angular.forEach(clipData.items, function (item, key) {
      if (clipData.items[key].type.match(/image.*/)) { // if it is a image
          files.push(clipData.items[key].getAsFile());
          $scope.attachmentHandler.onFileSelect(files);
      }
  });
}
```

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support6.webp)

Click the Save icon in the upper right corner and close the widget editor.

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support7.webp)

Go back to /sp_config and open the Designer

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support8.webp)

Open the "Ticket Form" page,

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support9.webp)

Hover the "Ticket Conversations" instance in the main window and click the little trash icon. This will not delete the widget, but only remove the instance of the widget. Next drag your newly created Widget to the place where the "Ticket Conversations" was.

That is it. You can now copy paste/images into the Ticket Conversation. Only requirement is that the user clicks the conversation before pasting.

You may also want to add the following business rule on the sys_attachment table to avoid the creation of files with the name "blob" without any extensions.

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support10.webp)

![Setting Admin overrides](/assets/images/post_service-portal-copy-paste-support11.webp)

I hope that you find this usefull. Any suggestions for improvements are welcomed 🙂
