---
author: LKH
title: "How to create SSH certificate for ServiceNow GIT integration"
---

Use the following command to generate a certificate that can be used to connect ServiceNow to GIT using SSH.

```bash
key ssh-keygen -t rsa -m PEM -b 4096 -C “email@address”
```

Replace **email@address** with the e-mail address / username that you use in your GIT repository.

Next paste the private key into your ServiceNow instance and add the pulbic key to your GIT login.