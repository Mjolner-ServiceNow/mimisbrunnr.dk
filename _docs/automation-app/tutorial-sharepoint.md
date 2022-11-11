---
layout: single
title: SharePoint Integration
permalink: /automation-app/sharepoint-integration/
last_modified_at: 2022-11-11T16:36:18-04:00
sidebar:
  nav: "aa"
toc: false
---

In this video tutorial we will show you how to integrate ServiceNow with SharePoint using Automation App.

{% include video id="5WCGxatY9aM" provider="youtube" %}

We us a bit of xml to enable access to sharepoint on the url https://XXXX-admin.sharepoint.com/

Here is the XML used:

```xml
<AppPermissionRequests AllowAppOnlyPolicy="true">
<AppPermissionRequest Scope="http://sharepoint/content/tenant"
Right="FullControl" />
</AppPermissionRequests>
```

In the video a script is also used to generate a certificate. Here is how it looks:

```powershell
param(
  [Parameter(Mandatory=$true)]
  [String] $certname,

  [Parameter(Mandatory=$true)]
  [String] $password
)

$certpw = ConvertTo-SecureString -String $password -Force -AsPlainText
$cert = New-SelfSignedCertificate -Subject "CN=$certname" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
$thumbprint = $cert.Thumbprint
$expire = $cert.NotAfter
Export-PfxCertificate -Cert $cert -FilePath "C:\temp\$certname.pfx" -Password $certpw
$pfxBytes = Get-Content "C:\temp\$certname.pfx" -Encoding Byte
$pftBase64 = [System.Convert]::ToBase64String($pfxBytes)

$oPem=new-object System.Text.StringBuilder
$oPem.AppendLine("-----BEGIN CERTIFICATE-----")
$oPem.AppendLine([System.Convert]::ToBase64String($cert.RawData,"InsertLineBreaks"))
$oPem.AppendLine("-----END CERTIFICATE-----")
$publicCert = $oPem.ToString()

Write-Output "Cert in PEM format to be uploaded to Azure App Registration (Save in .pem file and upload):"
Write-Output $publicCert
Write-Output " "
Write-Output "Private password protected PFX file in base64 format to be saved as encrypted variable in Azure Automation:"
Write-Output $pftBase64
Write-Output " "
Write-Output "The certificate is valid until $expire"
Write-Output "Tumbprint: $thumbprint"

Remove-Item -Path "Cert:\CurrentUser\My\$thumbprint" -DeleteKey
Remove-Item -Path "C:\temp\$certname.pfx"
```
