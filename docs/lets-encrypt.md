---
published: true
title: Let's Encrypt
layout: default
---

# generate wildcard cert
```bash
docker run \
-it \
--mount="type=bind,source=/data/,destination=/data/" \
-u $(id -u) \
--rm zerossl/client \
--key account.key \
--live \
--email "letsencrypt@domain.com" \
--curve prime256v1 \
--csr domain.com.csr \
--csr-key domain.com.key \
--crt domain.com.crt \
--domains "*.domain.com,domain.com" \
--generate-missing \
--handle-as dns \
--renew 3
```
