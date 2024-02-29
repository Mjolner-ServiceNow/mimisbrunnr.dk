---
layout: single
title: Automation App Security
excerpt: "Securing Your Automation App Setup: Best Practices for Connection, Authentication, and Authorization"
permalink: /automation-app/security/
last_modified_at: 2024-02-29,T10:10:18-04:00
sidebar:
  nav: "aa"
toc: true
---

In the digital age, securing automation tools is not just an option but a necessity. With increasing instances of security breaches, understanding how to protect your Automation App setup is vital. This guide delves into essential security considerations across three layers: connection, authentication, and authorization, providing you with a roadmap to a secure Automation App environment.

## Establishing a Secure Connection

At the heart of secure automation is the establishment of a protected connection. Automation App leverages HTTPS (Secure Hypertext Transfer Protocol), incorporating SSL/TLS (Secure Socket Layer/Transport Layer Security) protocols, to encrypt data transmitted to Microsoft Azure. This encryption ensures that sensitive information remains confidential, thwarting eavesdroppers' attempts to intercept data.

### HTTPS and SSL/TLS: A Shield Against Eavesdropping

When Automation App communicates with the Microsoft Azure management endpoint, it uses HTTPS to authenticate the endpoint's identity. This prevents impersonation by ensuring the endpoint presents a digital certificate, verified against trusted certificate authorities (CAs). Moreover, HTTPS maintains data integrity, making any unauthorized alteration detectable through cryptographic methods.

### OCSP: An Extra Layer of Trust

To enhance security, ServiceNow enables Online Certificate Status Protocol (OCSP) by default. OCSP checks the certificate's revocation status, adding an extra layer of security but also introducing a potential point of failure if the OCSP endpoint becomes unresponsive as this will cause ServiceNow to drop the connection. Balancing the benefits of OCSP against its risks is crucial in maintaining a secure and reliable connection.

## Authentication: Proving Your Identity

Once a secure connection is established, the next step is authentication—proving your identity to the endpoint.

### Option 1: Authenticate Using a Secret

Creating a secret in Microsoft Entra involves one-way encryption, meaning the secret is irreversible and only presented to you once. It's crucial to securely store this secret, as Microsoft Entra cannot retrieve it. 

Authentication involves sending the secret and Client ID over HTTPS, with Microsoft Entra verifying the secret's integrity. 

Remember, secrets have an expiry date, and managing their lifecycle is essential to avoid unintended downtime.

Best Practices for Managing Secrets:

- Regularly rotate secrets before their expiration.
- Securely store secrets to prevent unauthorized access.

### Option 2: Authenticate Using a Certificate

Certificates offer a higher security level, involving a pair of private and public certificates. Unlike secrets, the private certificate is never transmitted, minimizing the risk of exposure. Authentication with certificates involves signing data with the private certificate, with the public certificate used for verification by Microsoft Entra.

Best Practices for Certificate Management:

- Ensure certificates are securely stored and accessible only to authorized personnel.
- Regularly update certificates before their expiration to prevent service disruption.

## Authorization: Defining Access and Roles

With authentication out of the way, authorization determines what actions Automation App can perform in Microsoft Azure and what resources ServiceNow users can access.

### ServiceNow Roles and Permissions

Automation App comes with following build-in roles that you can assign your ServiceNow users:

- Automation App Reader: Offers read-only access to Runbooks.
- Automation App User: Allows working with Runbooks and other assets.
- Automation App Admin: Provides full access, including configuration settings.

Always have in mind that any user in ServiceNow with the **admin** role will be able to obtain any of the above roles.

### Microsoft Azure: Balancing Functionality and Security

Assigning the Contributor role in Azure unlocks full functionality but might not always be necessary. Customizing the Automation App's privileges can reduce security risks without sacrificing key capabilities.

For instance, if you want the Automation App to only start jobs on a specific Automation Account, you can achieve this by revoking the Contributor role and assigning only the Automation Operator role to that account.

It's important to note that editing an Automation Runbook can be done using tools other than the Automation App. This can be useful if you prefer not to allow your ServiceNow admins to edit certain Runbooks, or if you want to include team members who are more comfortable working directly in Visual Studio Code or other similar tools they're more accustomed to.


## Making Informed Security Decisions

Your security strategy should consider several key questions, from the use of OCSP to the assignment of roles in ServiceNow and Azure. These decisions should align with your organization's security posture and operational needs.

- Consider the trade-offs of enabling OCSP in ServiceNow.
- Decide between using a secret or certificate for authentication based on your security requirements.
- Manage the lifecycle of your authentication methods to balance security and operational efficiency.
- Carefully assign roles in ServiceNow and Azure to ensure the principle of least privilege.

## Conclusion

Securing your Automation App setup involves careful consideration of connection security, authentication methods, and authorization roles. 

By following best practices and making informed decisions, you can establish a robust security posture for your automation environment. For further guidance, refer to [Microsoft's security guidelines for Azure Automation](https://learn.microsoft.com/en-us/azure/automation/automation-security-guidelines).
