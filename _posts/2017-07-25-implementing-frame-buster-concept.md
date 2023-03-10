---
author: LKH
title: "Implementing Frame Buster concept on Service Portal "
---

The old content management system (CMS) of ServiceNow had a checkbox on content pages that (if checked) would cause the side to break any iframe and open the site using the entire browser window.

I needed to do this using the Service Portal instead for a customer as the service portal was loading with the nav_to.do and thus had the entire menu to the left visible after login. This was not desirable, so I looked into a Frame Buster (Also know as frame breaker or frame killer).

My approach was to create a Widget which I named "Frame Buster". In the "Body HTML template" I added the following code:

```html
<style> html{display:none;} </style>
<script>
    if(self == top || top.location.href.indexOf('sp_config') > -1) {
        document.documentElement.style.display = 'block';
    } else {
        top.location = self.location;
    }
</script>
```

The rest of the fields I left with their default value.

I then added the widget at the top of the home page of the Service Portal. This will cause the Service Portal to remove the iframe by reloading the page at top level in the browser if the page is not already at the top level and only of the page is not being displayed from a URL containing sp_config. The latter is important as the Brand Editor will otherwise stop working.

I hope that you find this useful.

Improvements suggestions or comments are welcomed!