# Guide: Generating RSA keypair with OpenSSL

OpenSSL is installed by default on most linux distributions and WSL.

```sh
 $ openssl genrsa -out private.pem 2048
 $ openssl rsa -in private.pem -outform PEM -pubout -out public.pem  
```

- Private Key (`private.pem`) - Use this for [signing JWTs](/README.md)
- Public Key (`public.pem`) - Provide this for onboarding  
