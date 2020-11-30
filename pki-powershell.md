# Public Key Infrastructure (PKI) - PowerShell

## Quick links

* [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pkiclient/new-selfsignedcertificate?view=win10-ps)

## Create Root Certificate (CA)

```
New-SelfSignedCertificate `
    -Type Custom `
    -Subject "CN=ca.example.com" `
    -FriendlyName "Example Root Certificate Authority (CA)" `
    -KeyAlgorithm 'ECDSA_nistP521' `
    -HashAlgorithm 'SHA256' `
    -KeyExportPolicy 'Exportable' `
    -NotAfter (Get-Date).AddYears(1) `
    -CertStoreLocation 'Cert:\CurrentUser\My' `
    -KeyUsage CertSign, CRLSign `
    -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2,1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.4,1.3.6.1.5.5.7.3.3,1.3.6.1.5.5.7.3.8","2.5.29.19={critical}{text}ca=TRUE")
```

## Create Code Signing Certificate

```
New-SelfSignedCertificate `
    -Type Custom `
    -Subject "CN=dev.example.com" `
    -FriendlyName "Example Code Signing Certificate" `
    -Signer cert:\CurrentUser\My\7AFE0C1D283BE7043C1B71CA35BA4C78DEA1AD97 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -KeyUsage DigitalSignature `
    -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

## List Certificates

```
Get-ChildItem -Path cert:\CurrentUser\my
Get-ChildItem -Path cert:\CurrentUser\root
Get-ChildItem -Path cert:\LocalMachine\my
Get-ChildItem -Path cert:\CurrentUser\root
```

## Export DER Certificate

```
Export-Certificate `
    -Cert cert:\CurrentUser\My\7AFE0C1D283BE7043C1B71CA35BA4C78DEA1AD97 `
    -FilePath "ca.example.com.cer"
```

## Export PFX Certificate

```
Export-PfxCertificate `
    -Cert cert:\CurrentUser\My\7AFE0C1D283BE7043C1B71CA35BA4C78DEA1AD97 `
    -FilePath "ca.example.com.pfx" `
    -Password (ConvertTo-SecureString -AsPlainText 'password' -Force)
```

## Import Certificate

```
Import-Certificate `
    -CertStoreLocation cert:\CurrentUser\root `
    -FilePath "ca.example.com.cer"
```
