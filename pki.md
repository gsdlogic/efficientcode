# Public Key Infrastructure (PKI)

[Home](index.md)

## Quick links

- [Microsoft Cryptography](https://docs.microsoft.com/en-us/windows/win32/seccrypto/cryptography-portal)
- [How to setup your own CA with OpenSSL](https://gist.github.com/Soarez/9688998)
- [Import and Export RSA Key Formats in .NET Core 3](https://vcsjones.dev/2019/10/07/key-formats-dotnet-3/)
- [The Most Common OpenSSL Commands](https://www.sslshopper.com/article-most-common-openssl-commands.html)
- [How to create a self-signed certificate in .NET Core](https://stackoverflow.com/questions/42786986/how-to-create-a-valid-self-signed-x509certificate2-programmatically-not-loadin)
- [Creating an X.509 Certificate Chain in C#](https://blog.rassie.dk/2018/04/creating-an-x-509-certificate-chain-in-c/)
- [CertificateUtil](https://github.com/rwatjen/AzureIoTDPSCertificates/blob/master/src/DPSCertificateTool/CertificateUtil.cs)
- [Create a certificate for package signing](https://docs.microsoft.com/en-us/windows/msix/package/create-certificate-package-signing)
- [Configure certificate authentication in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/certauth?view=aspnetcore-3.1#configure-your-server-to-require-certificates)
- [How to tell DV and OV certificates apart](https://unmitigatedrisk.com/?p=203)
- [CA/EV Processing for CAs](https://wiki.mozilla.org/CA/EV_Processing_for_CAs)
- [Configure certificate authentication in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/certauth?view=aspnetcore-6.0)

## Types of Certificates

- Root certificate (CA)
- Intermediate certificate (CA)
- Domain validation certificate (DV)
- Organization validation certificate (OV)
- Extended validation certificate (EV)
- Subject Alternative Name (SAN)
- Self-signed certificate
- User ID certificate
- Email certificate
- Code signing certificate

## Storage Formats

| Type     | Description                           | Encoding         | Extensions             |
|----------|---------------------------------------|------------------|------------------------|
| PEM      | Privacy-Enhanced Mail				   | Base64           | .pem, .crt, .cer, .key |
| PKCS #7  | Public Key Cryptography Standards #7  | Binary or Base64 | .p7b, .p7c			   |
| PKCS #12 | Public Key Cryptography Standards #12 | Binary			  | .pfx, .p12			   |
| DER	   | Distinguished Encoding Rules          | Binary           | .der, .cer             |

## Policies

| Type                                     | Policy Identifier |
|------------------------------------------|-------------------|
| Domain validation certificate (DV)       | 2.23.140.1.2.1    |
| Organization validation certificate (OV) | 2.23.140.1.2.2    |
| Extended validation certificate (EV)     | 2.23.140.1.1      |

## Implementations

* [OpenSSL](pki-openssl.md)
* [PowerShell](pki-powershell.md)
* [.NET](pki-dotnet.md)
