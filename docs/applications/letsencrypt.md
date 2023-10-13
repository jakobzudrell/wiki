---
title: Let's Encrypt
summary: Let's Encrypt guides
authors:
    - Jakob Zudrell
date: 2023-10-13
---
# Let's Encrypt
A nonprofit Certificate Authority, providing your with TLS certificates to all your services.

## Manual Certificate via DNS
```console
sudo certbot certonly -d yourdomain --manual --preferred-challenges dns
```