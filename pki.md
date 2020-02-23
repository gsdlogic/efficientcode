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

## Types of Certificates

- Root certificate (CA)
- Intermediate certificate (CA)
- Domain validation certificate (DV)
- Extended validation certificate (EV)
- Subject Alternative Name (SAN)
- Self-signed certificate
- User ID certificate
- Email certificate
- Code signing certificate

## Storage Formats

- CER (Certificate)
- DER (Distinguished Encoding Rules)
- PEM (Privacy-Enhanced Mail)
- PKCS #7 (Public Key Cryptography Standards #7)
- PKCS #12 (Public Key Cryptography Standards #12)

## RSA Key Pair

### OpenSSL

To generate a PKCS#1 PEM encoded file, use

```
openssl genrsa -out private.key
```

To view the contents of the private key, use:

```
openssl rsa -in private.key -noout -text
```

To export the RSA public key, use:

```
openssl rsa -in private.key -out public.key -RSAPublicKey_out
```

To view the contents of the RSA public key, use:

```
openssl rsa -in public.key -pubin -RSAPublicKey_in -text
```

### .NET Core

To import the private key in RSA PKCS#1 format, convert the Base-64 encoded key to binary and use:

[RSA.ImportRSAPrivateKey](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.importrsaprivatekey?view=netcore-3.1)

To import the public key in RSA PKCS#1 format, convert the Base-64 encode key to binary and use:

[RSA.ImportRSAPublic](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.importrsapublickey?view=netcore-3.1)

To export the private key in RSA PKCS#1 format, use:

[RSA.ExportRSAPrivateKey](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.exportrsaprivatekey?view=netcore-3.1)

To export the public key in RSA PKCS#1 format, use:

[RSA.ExportRSAPublicKey](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.exportrsapublickey?view=netcore-3.1)

## Certificate Signing Request (CSR)

### OpenSSL

To generate a CSR:

```
openssl req -new -key example.org.key -out example.org.csr
```

The following information will be requested from standard input:

- Country Name (2 letter code)
- State or Province Name (full name)
- Locality Name (eg, city)
- Organization Name (eg, company)
- Organizational Unit Name (eg, section)
- Common Name (eg, fully qualified host name)
- Email Address
- A challenge password

For an example with SubjectAltName, see [How to setup your own CA with OpenSSL](https://gist.github.com/Soarez/9688998).

To view the CSR contents:

```
openssl req -in example.org.csr -noout -text
```

### .NET Core

To generate a certificate request, use:

[CertificateRequest.CreateSigningRequest](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.x509certificates.certificaterequest.createsigningrequest?view=netcore-3.1)

Example:

```
var request = new CertificateRequest(
    new X500DistinguishedName("CN=example.org"),
    RSA.Create(2048),
    HashAlgorithmName.SHA256,
    RSASignaturePadding.Pkcs1);

var csr = request.CreateSigningRequest();
```

The [CertificateRequest](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.x509certificates.certificaterequest?view=netcore-3.1) is the base class used for creating certificates.

See also:

- [CertificateRequest.Create](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.x509certificates.certificaterequest.create?view=netcore-3.1)
- [CertificateRequest.CreateSelfSigned](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.x509certificates.certificaterequest.createselfsigned?view=netcore-3.1)
