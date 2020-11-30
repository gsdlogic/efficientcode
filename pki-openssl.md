# Public Key Infrastructure (PKI) - OpenSSL

## RSA Key Pair

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

## Certificate Signing Request (CSR)

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

## Convert PEM certificate and key to PFX

```
openssl pkcs12 -inkey key.pem -in cert.pem -export -out cert.pfx
```

## Export PFX private key to PEM format

```
openssl pkcs12 -in cert.pfx -nocerts -nodes -out key.pem
```
 
## Convert DER certificate to PEM format

```
openssl x509 -inform DER -in cert.cer -out cert.pem
```
