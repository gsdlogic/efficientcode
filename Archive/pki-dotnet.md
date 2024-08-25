# Public Key Infrastructure (PKI) - .NET

## RSA Key Pair

To import the private key in RSA PKCS#1 format, convert the Base-64 encoded key to binary and use:

[RSA.ImportRSAPrivateKey](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.importrsaprivatekey?view=netcore-3.1)

To import the public key in RSA PKCS#1 format, convert the Base-64 encode key to binary and use:

[RSA.ImportRSAPublic](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.importrsapublickey?view=netcore-3.1)

To export the private key in RSA PKCS#1 format, use:

[RSA.ExportRSAPrivateKey](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.exportrsaprivatekey?view=netcore-3.1)

To export the public key in RSA PKCS#1 format, use:

[RSA.ExportRSAPublicKey](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rsa.exportrsapublickey?view=netcore-3.1)

## PEM Encoding

Use the following table to map PEM encoded files to .NET Core methods:

| PEM Label             | Type  | Method                         |
|-----------------------|------ |--------------------------------|
| RSA PRIVATE KEY       | RSA   | ImportRSAPrivateKey            |
| PRIVATE KEY	        | RSA   | ImportPkcs8PrivateKey          |
| ENCRYPTED PRIVATE KEY | RSA   | ImportEncryptedPkcs8PrivateKey |
| RSA PUBLIC KEY	    | RSA   | ImportRSAPublicKey             |
| PUBLIC KEY            | RSA   | ImportSubjectPublicKeyInfo     |
| EC PRIVATE KEY        | ECDsa | ImportECPrivateKey             |

Given the following PEM encoded file:

```
-----BEGIN EC PRIVATE KEY-----
abcde...
-----END EC PRIVATE KEY-----
```

Import the file using the following code:

```
var key = Convert.FromBase64String("abcde...");
var ecdsa = ECDsa.Create();
ecdsa.ImportECPrivateKey(key, out _);
```

Given a PEM encoded certificate:

```
-----BEGIN CERTIFICATE-----
abcde...
-----END CERTIFICATE-----
```

Load a certificate with the key:

```
var cert = new X509Certificate2(Convert.FromBase64String("abcde..."))
    .CopyWithPrivateKey(key);
```

## Certificate Signing Request (CSR)

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
