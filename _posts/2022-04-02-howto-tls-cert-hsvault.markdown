---
layout: post
title:  "Howto create self-signed SAN TLS cert for HSVault"
date:   2022-04-02 07:44:00 +0800
categories: [security]
---
### Requirement
- Create a self-signed SAN Certificate for HS Vault TLS communication

### Method
- openssl

#### Flow
- Download and install openssl if it is not in the system
- Search system for openssl.cnf, copy it to a work directory
- Add alternate name to the openssl.cnf
- Create CA-Key & Cert
- Create TLS Key
- Create TLS Cert

#### Procedure
- Search system for openssl.cnf, copy it to a work directory

```
╰─○ find / -name openssl.cnf 2> /dev/null
/usr/local/etc/openssl@1.1/openssl.cnf

cp /usr/local/etc/openssl@1.1/openssl.cnf /tmp
```

- Add alternate name to the openssl.cnf

```
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = vault.int.qool.one
DNS.2 = vault01.int.qool.one
DNS.3 = vault02.int.qool.one
DNS.3 = vault03.int.qool.one
DNS.4 = localhost
IP.1 = 127.0.0.1
```


- Create CA-Key & Cert

```
╰─○ openssl genrsa -aes256 -out ca.key 4096
Generating RSA private key, 4096 bit long modulus
..............................................................................................................................................................++
.......................................................++
e is 65537 (0x10001)
Enter pass phrase for ca.key:
Verifying - Enter pass phrase for ca.key:

╰─○ openssl req -x509 -new -nodes -key ca.key -sha256 -days 10950 -out ca.pem -subj "/C=MY/ST=WP/L=KL/O=QoolInc/OU=Qool/CN=Qool Certificate Authority"
Enter pass phrase for ca.key:

╰─○ openssl x509 -enddate -noout -in ./ca.pem
notAfter=Dec 26 05:15:36 2051 GMT
```

- Create TLS Key & CSR

```
╰─○ openssl req -new -nodes -newkey rsa:4096 -keyout tls.key -out tls.csr -batch -subj "/C=MY/ST=WP/L=KL/O=QoolInc/OU=qool/CN=qool.one"
Generating a 4096 bit RSA private key
..................................................................................................................................................++
..........................................................................................................................................................................................................................................++
writing new private key to 'tls.key'
-----
```

- Create TLS Cert

```
╰─○ openssl x509 -req -in tls.csr -CA ca.pem -CAkey ca.key -CAcreateserial -out tls.crt -days 3650 -sha256 -extensions v3_req -extfile ./openssl.cnf
Signature ok
subject=/C=MY/ST=WP/L=KL/O=QoolInc/OU=qool/CN=qool.one
Getting CA Private Key
Enter pass phrase for ca.key:

╰─○ openssl verify -verbose -CAfile ca.pem tls.crt
tls.crt: OK

╰─○ openssl x509 -text -noout -in tls.crt | grep DNS:
                DNS:vault.int.qool.one, DNS:vault01.int.qool.one, DNS:vault03.int.qool.one, DNS:localhost, IP Address:127.0.0.1
```

### Limitation
