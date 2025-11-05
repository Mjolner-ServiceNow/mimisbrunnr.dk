---
layout: splash
author_profile: false
permalink: "/"
title: "Mimis Brunnr - the well of wisdom"
header:
  overlay_image: /assets/images/header-bg.webp
excerpt: "Here you will find all the documentation you need for our ServiceNow apps"
aa_row:
  - image_path: /assets/images/logo-aa-orange.svg
    alt: "Automation App logo"
    title: "Automation App"
    excerpt: "Automate anything - cloud or on-prem - directly in ServiceNow with the Automation App. Use PowerShell or Python with Flow Designer for fast, no-code workflows and full visibility. Enjoy bi-directional integration, ROI reporting, and unlimited use at a flat rate. No prerequisites. Just powerful, seamless automation."
    url: /automation-app
    btn_label: "Learn more"
    btn_class: "btn--inverse" 
fa_row:
  - image_path: /assets/images/logo-fa-orange.svg
    alt: "Feedback App logo"
    title: "Feedback App"
    excerpt: "Enhance your Service Portal with the Feedback App - an easy-to-use, customizable widget for collecting user insights. Supports Like/Dislike, star ratings, or NPS. Feedback is stored for reporting or tied to records like Incidents. Works across portals, supports file uploads, and offers help/bug buttons for a seamless, user-friendly experience."
    url: /feedback-app
    btn_label: "Get started"
    btn_class: "btn--inverse"
ia_row:
  - image_path: /assets/images/logo-ia-orange.svg
    alt: "Integration App logo"
    title: "Integration App"
    excerpt: "The Integration App enables seamless ticket and configuration data exchange between your ServiceNow instance and your Service Providers - whether they use ServiceNow or another platform. It supports the synchronization of Incidents, Changes, and Configuration Items, making it ideal for managed service environments, multi-vendor setups, or enterprises with multiple ServiceNow instances."
    url: /integration-app
    btn_label: "Learn more"
    btn_class: "btn--inverse"
rma_row:
  - image_path: /assets/images/logo-rma-orange.svg
    alt: "Rights Management App logo"
    title: "Rights Management App"
    excerpt: "The Rights Management App lets you manage both Active Directory and Entra ID seamlessly from ServiceNow. Use Azure Automation and no-code tools like Flow Designer and Workspace to simplify identity tasks. Gain centralized access, full integration, and streamlined control - all from a single, user-friendly ServiceNow workspace."
    url: /rights-management-app
    btn_class: "btn--inverse"

---

{% include feature_row id="aa_row" type="left" %}
{% include feature_row id="fa_row" type="right" %}
{% include feature_row id="ia_row" type="left" %}
{% include feature_row id="rma_row" type="right" %}
