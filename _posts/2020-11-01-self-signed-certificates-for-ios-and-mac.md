---
layout: post
title: 'Generate self-signed certificates for newest iOS/macOS versions'
tags: ios macos self-signed certificates apple
comments: true
permalink: self-signed-certificates
share: true
---

Because of the `!new` Apple [Requirements for trusted certificates in iOS 13 and macOS 10.15](https://support.apple.com/en-us/HT210176) self-signed certificates need to be created in a different way to comply with the follow requirements:

```
- TLS server certificates and issuing CAs using RSA keys must use key sizes greater than or equal to 2048 bits. Certificates using RSA key sizes smaller than 2048 bits are no longer trusted for TLS.
- TLS server certificates and issuing CAs must use a hash algorithm from the SHA-2 family in the signature algorithm. SHA-1 signed certificates are no longer trusted for TLS.
- TLS server certificates must present the DNS name of the server in the Subject Alternative Name extension of the certificate. DNS names in the CommonName of a certificate are no longer trusted.

Additionally, all TLS server certificates issued after July 1, 2019 (as indicated in the NotBefore field of the certificate) must follow these guidelines:

- TLS server certificates must contain an ExtendedKeyUsage (EKU) extension containing the id-kp-serverAuth OID.
- TLS server certificates must have a validity period of 825 days or fewer (as expressed in the NotBefore and NotAfter fields of the certificate).
```

And we need a valid certificate because 

```
Connections to TLS servers violating these new requirements will fail and may cause network failures, apps to fail, and websites to not load in Safari in iOS 13 and macOS 10.15.
```

So, here is what we need to do:

## Create a CA Root certificate:

```bash
$ openssl genrsa -out ca.key 4096

$ openssl req -x509 -new -nodes -sha256 -days 365 -key ca.key -subj "/O=Development/CN=Development Root CA" -out ca.crt
```

## Add CA Root certificate to the system trust zone

You need to add the CA Root certificate to the trust zone on (iOS)[https://support.apple.com/en-us/HT204477] and (macOS)[https://support.apple.com/guide/keychain-access/change-the-trust-settings-of-a-certificate-kyca11871/mac]


## Create the Server certificate

```bash
$ openssl req -newkey rsa:4096 -nodes -keyout server.key -subj "/O=Development/CN=*.local.dev" -out server.csr

$ openssl x509 -req -sha256 -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -extfile <(printf "subjectAltName=DNS:*.local.dev")
```

## Extra: Create PKCS12 keystore if needed for your server

```bash
$ openssl pkcs12 -export -in server.crt -inkey server.key -certfile server.crt -out server.p12
```

## Configue your Server with the new certificate

For example to enable HTTPS in Spring Boot and use the self-signed certificate, you can use something like:

```yaml
server.port=8443

server.ssl.enabled=true
server.ssl.key-store=classpath:server.p12
server.ssl.key-store-password=123123
server.ssl.key-alias=1
server.ssl.key-store-type=PKCS12

security.require-ssl=true
```

## Check if generated Server certificate pass App Transport Security Diagnostics

```bash
$ nscurl --ats-diagnostics https://subdomain.local.dev
```

## END

The App Transport Security Diagnostics should show a lot of `Result : PASS` test statuses

Which means that the iOS/Mac device is connecting to the desired endpoint without showing any warning, first because the CA Root is trusted and the Server certificated was issued by a trusted CA Root, and second, because its a valid certificate that follow new Apple requierements for trusted certificates

And thats all folks.
