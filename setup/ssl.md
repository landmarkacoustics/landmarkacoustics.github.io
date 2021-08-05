---
title: Self-signed Certificates for Testing
author: Ben Taft
copyright: 2020 by Landmark Acoustics LLC
---

# Self-signed Certificates for Testing

These instructions distill the tutorial [here][1].

## Generate a server key.

-  The method that the tutorial suggested is:
```console
openssl genrsa -out server.key -des3 -passout file:passfile 1024
```
-  The man page for `openssl` suggestes a newer utility:
```console
openssl genpkey \
    -out server.key \
    -des3 -pass file:passfile \
    -algorithm RSA \
	-pkeyopt rsa_keygen_bits:2048 \
```
-  The options/arguments have the same meaning for each command.

   `-out server.key`
   :  This, of course, is the output file name and should be changed accordingly.
    
   `genrsa` or `-algorithm RSA`
   :  Use the RSA algorithm to create the key.

   `-des3`
   :  The output will be encrypted using a "Triple-DES" cipher.

   `-passout file:passfile` or `-pass file:passfile`
   :  A file that contains the passphrase as its first line.

   `2048` or `-pkeyopt rsa_keygen_bits:2048`
   :  The size of the RSA key in bits.  This sets it to 2048

## Generate a request to sign a certificate (CSR).
-  The command seems to be
```console
openssl req \
	-new \
	-key server.key \
	-out server.csr \
	-config cert.cfg \
	-passin file:passfile
```
-  The options are

   `-key server.key`
   :  The private key generated in the previous step.
   
   `-out server.csr`
   :  The file that contains the request to sign the certificate.

   `-config cert.cfg`
   :  The file that contains certificate configuration info.
   
      This file should look like the code block below.
	  The really critical part is setting the CommonName to 'localhost'.

```config
[req]

prompt             = no
distinguished_name = req_distinguished_name

[req_distinguished_name]

C                  = US
ST                 = WI
L                  = Racine
O                  = Landmark Acoustics LLC

CN                 = localhost
```

   `-passin file:passfile`
   :  The same password file used in the previous step.

## Sign the certificate yourself.
-  The command seems to be
```console
openssl x509 -req \
    -days 1024 \
-in server.csr -passin file:passfile \
    -signkey server.key -out server.crt
```
-  the options are
   `-req`
   :  sign a CSR

   `-days 1024`
   :  the lifespan for the certificate
   
   `-in server.csr`
   :  the request to sign the certificate that was created in the previous step
   
   `-signkey server.key`
   :  the private key for the server that was created in the first step
   
   `-passin file:passfile`
   :  The password file for the private key
   
   `-out server.crt`
   :  The output file that will contain the signed certificate.
   

[1]:https://developer.salesforce.com/blogs/developer-relations/2011/05/generating-valid-self-signed-certificates.html
