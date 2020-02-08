# RSA

## Create private key

```bash
openssl
genrsa -out rsa_private_key.pem 1024
```

### Change RSA to PKCS8

```bash
openssl
pkcs8 -topk8 -inform PEM -in rsa_private_key.pem -outform PEM -nocrypt
```

## Create public key

```bash
openssl
rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem
```



