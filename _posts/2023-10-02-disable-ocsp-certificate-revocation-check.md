---
author: LKH
title: "How to disable the OCSP certificate revocation check"
---

When using Automation App you may experience OCSP issues in your log files like the one below.

```
[...] org.apache.commons.httpclient.HttpException: unknown tag 28 encountered
```

These errors are caused by the OCSP responder returning a 503 Service Unavailable. 

This will make you unable to connect from ServiceNow to Microsoft Azure. The error may be more or less persistant and will go away once the OCSP responser becomes available again.

## What is OCSP?

OCSP stands for Online Certificate Status Protocol. It is a protocol used  to check the status of digital certificates in real-time. In ServiceNow we use it to verify the authenticity of SSL/TLS encrypted connections.

Here's how OCSP works in ServiceNow:

1. When ServiceNow connects to a server secured with SSL/TLS, the server presents its digital certificate to prove its identity.

2. ServiceNow may need to verify the certificate's status to ensure that it has not been revoked or expired. This is crucial for security because a revoked or expired certificate may indicate a compromised server.

3. Instead of downloading a list of all revoked certificates (Certificate Revocation List or CRL) and checking if the server's certificate is on that list, ServiceNow uses OCSP.

4. ServiceNow sends a request to the OCSP responder, which is a server operated by the certificate authority (CA) that issued the certificate. This request contains the certificate in question.

5. The OCSP responder checks its database to see if the certificate is still valid or if it has been revoked. It then sends a response to the client, indicating whether the certificate is "good," "revoked," or "unknown."

5. Based on the OCSP response, ServiceNow can make an informed decision about whether to trust the server's certificate.

OCSP provides a more efficient and real-time method for checking the status of certificates compared to CRLs, which can be large and require periodic updates. 

## Why is OCSP a challange?

OCSP has its own challenges, such as potential privacy concerns, since it will require ServiceNow to query the CA's server.

More critically it provides reliability issues if the OCSP responder is unavailable, which is the reason why you are seeing the error mentioned above.

## How to disable OCSP

Fortunately it is possible to disable OCSP on your ServiceNow instance. You can do this by adding the sys_property **com.glide.communications.httpclient.verify_revoked_certificate** and setting it to **false**.

Notice that this will disable OCSP system wide and not just for Automation App.

When OCSP is disabled the certificate is still validated, but ServiceNow will no longer ask the CA if the certificate has been revoked. There is a security risk in this, but, depending on your setup, there is also a risk in relying on the OCSP responder being available.

For more information please refer to this article on [ServiceNow Docs](https://docs.servicenow.com/en-US/bundle/vancouver-platform-security/page/administer/security/reference/verify-revoked-certificate.html).