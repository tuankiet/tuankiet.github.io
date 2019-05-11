---
layout: default
title: Install SSL Namecheap for NGINX server
date: 2018-09-12
categories: ruby
---

# Install SSL Namecheap for NGINX server

In case of Comodo certificates

- receive the zip archive with `*.crt` and `*.ca-bundle` files then extract it

- you will receive files:

  - youdomainname.crt

  - youdomainname.ca-bundle

- NGINX need all the certificates. You should combine theses 2 files. The `*.crt` should be list first, followed by (theo sau) the chain of CA certificates `*.ca-bundle`

- Note: CMD combine: `$ cat yourdomainname.crt yourdomainname.ca-bundle >> cert_chain.crt`

- Edit your nginx configure file like this one:

```
server {
listen 443;
ssl on;
ssl_certificate /etc/ssl/cert_chain.crt;
ssl_certificate_key /etc/ssl/yourdomainnamekey.key;
}
```

-  But from NGINX version 1.15.0 your nginx configure change like this one:

```
server {
listen 443 ssl;
ssl_certificate /etc/ssl/cert_chain.crt;
ssl_certificate_key /etc/ssl/yourdomainnamekey.key;
}
```

- Starting from Nginx 1.15.0 it is also possible to set up a single HTTP/HTTPS server:

```
server { 
listen 80;
listen 443 ssl;
ssl_certificate /etc/ssl/cert_chain.crt;
ssl_certificate_key /etc/ssl/yourdomainnamekey.key; 
}
```

- Reload & Restart NGINX: `sudo service nginx restart`  for linux or `brew services restart nginx` for mac



