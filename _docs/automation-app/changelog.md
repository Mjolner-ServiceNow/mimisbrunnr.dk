---
layout: single
title: Changelog
permalink: /automation-app/changelog/
#last_modified_at: 2022-10-25T08:16:18+01:00
sidebar:
  nav: "aa"
toc: true
---

## 3.0.6 October 11th, 2023

We're thrilled to introduce Automation App Version 3.0, loaded with exciting new features, crucial bug fixes, and some tech upgrades to elevate your automation experience. Here's the lowdown on what's changed:

New Features:

- 📊 Runbook Insights: Stay in the know with real-time updates on completion, failure, and expected runtime for each Runbook.
- 📚 Module Access: Access modules directly from the Runbook Manager UI for seamless workflow management.
- 🔄 Runbook Version Comparison: Easily compare your Draft with the Published version of the Runbook.
- 🔍 Enhanced Search: Enjoy improved search capabilities in variables, certificates, and credentials.
- 📖 Documentation and Support Links: Access documentation and support resources directly within Runbook Manager.
- ⚙️ Runtime Exceptions: Identify and address runtime exceptions right from Runbook Manager.
- 🛠️ One-Step Runbook Creation: Create a Runbook from a Template in a simplified one-step process.
- ⚙️ Account Settings: Access Automation Account settings conveniently from Runbook Manager.
- 🌐 Integration with Polaris UI: Seamlessly open Runbooks in Runbook Manager from the ServiceNow Polaris UI.
- ✂️ Efficient Snippets: Snippets are automatically pasted at the current cursor position in Runbook Manager for enhanced productivity.

Bug Fixes:

- 🐛 Speedy Module Imports: Fixed slow import issues for larger modules, ensuring a smoother experience.
- 🌐 Azure Patch Management Fix: Resolved the issue where Patch Management Jobs in Azure caused excessive load during import into ServiceNow.

Technical (Nerdy) Changes:

- ⬆️ React Upgrade: Leveled up from React 16 to 18 for a more powerful and responsive user interface.
- 🆙 Bootstrap Enhancement: Upgraded from Bootstrap 4 to 5, bringing a modernized design and improved responsiveness.
- 🚀 ECMAScript Boost: Uplifted from ES5 to ES12 for enhanced performance and compatibility.

We're dedicated to continuously improving Automation App to make your automation journey smoother and more efficient. We hope you enjoy these enhancements, and stay tuned for more exciting updates in the future! 🎉

## 2.7.2 January 10th, 2023

In this release we have focused on security and stability. To add to the security of the communication between Automation App and Azure we now support OAuth 2.0 certificate-based authentication as an alternative to using a secret 😎

You are now also able to manage certificates in your Automation Account directly from Runbook Manager making it even easier for you to secure your integrations by using certificates 💪

We have also improved the syntax highlighting and output handling of PowerShell 7 based Runbooks 🤩

Lastly, we have made improvements to make the execution engine even more robust to cater for situations where unexpected results caused by network errors or endpoint unavailability would break communications 🙌

## 2.6.1 September 8th, 2022

We now capture and save the actual runtime of Jobs. You can use this for reporting, but we are also using this information to optimize when we are fetching updates on running Jobs. This gives you faster execution times and extended possibilities for accurate reporting 🙌

To keep your ServiceNow data accurate it is now also possible to delete Runbooks that have not been discovered (hence removed in Azure) 😊

Also we have been on a bug hunt and removed a bug that would truncate the output of Jobs with more than 100 lines of output 😎

This update is recommended for everybody. Thank you all for the positiv feedback and all the great ideas that you are submitting 🤩

## 2.5.5 April 13th, 2022

This release includes new and improved features 🤩

PowerShell 7.1 is now officially supported in Runbook Manager giving you the option to work with this preview feature of Azure Automation 👍

Extension based Hybrid Worker Groups are now supported giving you an even easier way of adding Hybrid Workers that are based on Azure or Azure Arc 😎

In Runbook Manager we have added the posibility to test and execute jobs. This makes it much easier for you to test and evaluate your Runbooks directly from ServiceNow 🙌

We have also removed a few bugs as well as implemented performance and UI improvements making this the best version of Automation App ever 💪

Remember to check the Upgrade History in the Application Navigator to ensure that no changes were skipped 😊

## 2.4.1 October 20th, 2021

This release contains both bug fixes as well as new features 🤩

A brand new Dashboard combined with the option of adding manual lead time and effort on your automation jobs gives you the possibility to show case just how much your automation effort is saving your organization 🏆

In Runbook Manager we have added the possibility to both update as well as delete credentials and variables. This makes it much easier for you to work with your automation account assets directly from ServiceNow 🙌

Python3 is now officially supported in Runbook Manager giving you the option to work with this preview feature of Azure Automation 👍

We have also removed a few bugs as well as implemented performance and UI improvements making this the best version of Automation App ever 💪

## 2.3.2 April 13th, 2021

We do not like bugs, but we have found one! 😳 And we have removed it. 💪

This release contains a bug fix that fixes an issue where System Hybrid Workers would get imported for an Automation Account even though the import of System Hybrid Workers was not selected. 🙈

## 2.3.0 March 25th, 2021

Now launching Runbook Alias 🎉. A new function created based on the feedback from you! 💪

How do I ensure, that when I execute a flow in my ServiceNow development instance, that I am always using the Runbook in my Azure development Automation Account? How do I move the flow to my ServiceNow test instance, without modifying the flow, and still ensure that I am now hitting a Runbook in my Azure test Automation Account? And how do I get this to production? 🤔

The answer is Runbook Alias. With Runbook Alias you can use the Automation App to define an Alias for your Runbooks and define which ServiceNow instance should use which Azure Runbook. Thereby the Automation App will, based on the ServiceNow instance that a flow is executed on, automatically pick the correct Runbook. 🤩

We hope that you are going to love this new feature. Please let us know what you think, and make sure to keep sending us suggestions for new improvements and features. We love to hear from you! 😍

## 2.2.7 February 9th, 2021

Fixes a bug that could cause javascript issues on Service Portal

## 2.2.6 February 5th, 2021

Minor bug fixes

## 2.2.0 January 11th, 2021

We value your input and are happy to release a new update to the Automation App. We hope that you will enjoy the new features and improvements in this release.

New features and improvements

- Archive function to automatically remove input and output values from Jobs after a configurable amount of time to ensure compliance with GDPR and other policies while keeping the ability to do reporting on your automation.
- It is now possible to request update on the status of a Job ad hoc using a new “Refresh” UI Action on the Job form.
- If a PowerShell Runbook has an input variable of type String, the input value will automatically be wrapped in double quotes if quotes are missing.
- New “Read Only” role to allow access to users who should have access to read Jobs, but not have access to modify Runbooks etc.
- New Flow Designer Action to more easily retrieve Output Variables from a completed Job.
- A log tab has now been added to the Job form showing all communication regarding the Job for ease of troubleshooting.
- It is now possible to create Credentials from Runbook Manager.
- Link on the Application form in ServiceNow to the Application in the Microsoft Azure Portal.
- Import of Automation Accounts will now automatically start after an Application has been created.
- For MSP instances (Domain Separation) it is now possible to allow records in the global domain.

Bug fixes and stability

- Fixed a bug where only the first 20 Runbook Template categories would display in Runbook Manage.
- The removal of a Microsoft Azure Tenant is now run in the background to avoid timeout when larger Tenants were removed.
- In some cases, Jobs would get stuck in status “Running” if they were deleted in Azure by an administrator or if access to the Job was removed.
- If Credentials were deleted in Azure this could make the subsequent import in ServiceNow fail.
- If connection to Azure was lost during an import the App would in some cases remove multiple assets from ServiceNow.
- If more than one Application was registered to the same Microsoft Azure Tenant, the last registered Application would fail to import.

## 2.1.0 November 30th, 2020

- Minor bug fixes
- Adds the possibility to use GitHub based templates

## 2.0.32 July 1st, 2020

Minor bug fixes and performance improvements

## 2.0.26 June 24th, 2020

- Added possibility to set import properties on Automation Account level.
- Added possibility to disable job import to ensure that ServiceNow only contains jobs created by ServiceNow.
- Fixed issue where importing more than 1000 jobs would sometimes cause timeout error.

## 2.0.20 May 14th, 2020

- Removed no longer supported ”Run as” field
- Increased width of snippet section in Runbook Manager for a better overview of available CMDLETS

## 2.0.19 March 31st, 2020

- Fixed bug where only the first 20 Runbooks of each Automation Account would display in Runbook Manager
- Fixed unresponsive "Show more" function on variables in the snippet section of Runbook Manager

## 2.0.1 March 20th, 2020

- Brand new Runbook Manager UI for managing runbooks
- Modules including their activities, parameter sets and paramaters are now being discovered
- Code snippets for cmdlets, variables, credentials and runbooks are now available directly in the runbook editor
- Improved domain seperation support
- Copy script snippets directly from the list of runbooks in Runbook Manager for easy implementation in workflows
- Improved performance for syncronization with Microsoft Azure
- Jobs in state "Suspended" and "Stopped" are now considered inactive to ensure better transparency of flows and workflows that need attention

## 1.3.12 November 20th, 2019

Bug fixes
- Installation error introduced in 1.3.9 where start job action was corrupted has now been fixed.

## 1.3.9 October 23rd, 2019

New Features
- It is now possible to delete Runbooks from ServiceNow.
- UI for Runbook creation has been improved.
- Pull Runbook content from Azure on demand.

Bug fixes
- Bug where it was not possible to remove an old tenant if connection to that tenant was lost has now been fixed.

## 1.3.8 October 3rd, 2019

New Features
- Documentation is now embedded in the app and accessible via the application navigator.
- Documentation has been updated to reflect recent UI changes in Microsoft Azure and now also includes PowerShell examples and recommendations.
- Support for enabling and disabling Log Verbose mode on Runbooks.

Bug fixes
- The output field now correctly shows the output from Azure when an object is returned.

## 1.2.16 September 11th, 2019

Bug fix: Fixed issue with cross-scope privileges on ServiceNow New York release.

## 1.2.14 June 19th, 2019

- Improved output handling.
- Possibility to automatically parse JSON output from job.

## 1.2.10 May 2nd, 2019

- Improved functionality when using Flow Designer.
- Auto detect input format for ease of use.
- Possibility to select if variables, user hybrid workers and system workers should be imported to your ServiceNow instance.

## 1.1.15 April 1st, 2019

Improved functionality when using Flow Designer

## 1.1.11 March 12th, 2019

Performance increase by adding dynamic update interval.

## 1.1.9 December 11th, 2018

Increased security by adding extra constraints on UI actions and ACLs

## 1.1.8 November 8th, 2018

Bug fix: Fixed issue where Runbooks on import were set to "Edit" state
Bug fix: Fixed issue with duplicate form sections

## 1.1.7 September 21st, 2018

Full Domain Seperation support

## 1.1.6 September 14th, 2018

London compatible version

## 1.0.1 September 13th, 2018

Fixed issue where runbooks were not started correctly when hybrid worker group (run on) and run as were set.

## 1.0.0 September 4th, 2018

Initial release certified for Kingston
