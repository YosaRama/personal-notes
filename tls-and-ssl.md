---
description: >-
  describe what is TLS and SSL and how it works, what the purpose to use SSL and
  how to implement that
---

# TLS and SSL

## Problem

When a deploy a website into the server, it will be `http://example.com` and that means that's website is not secure. how can we setup `https` for that website?

## Approach

We need to install SSL/TLS Certificate to enable HTTPS

SSL is Secure Socket Layer it's a standard technology to securing an internet connection between browser and website it self (2 server connection)

SSL Certificate (digital certificate) are establish encrypted connection between user machine and website/remote machine

SSL is already deprecated now it's called TLS (Transport Layer Security) which is TLS is the same with SSL but it's more newer than SSL it self. when someone talk about SSL it's refer to TLS Protocol and TLS Certificate. TLS also used for handshake between 2 server in a secure way.

What is different between SSL and TLS? it will be only about how it's doing handshake, SSL will do explicit ways, and TLS it will doing in implicit ways. and also TLS have other alert message (warning for error occurred but connection still continue, fatal thats means terminated connection, and close notify alert about end of the session)

The benefit to have SSL certificate on your website is for make sure the transfer data always in encryption method, when we have SSL/TLS certificate that's mean transfer information will be pass into HMACs algorithm and make it more secure.

## Implementation

There are a lot of Free SSL Certification, CloudFlare & Let's Encrypt is the most popular ones. here is the example to use Let's Encrypt. let's say we used **Ubuntu** and **Apache** on the server

1. Install Snapd

```
sudo apt update
sudo apt install snapd
```

2. Remove certbot-auto any Certbot pacakge on the OS

```
sudo apt-get remove certbot
```

3. Install certbot

```
sudo snap install --classic certbot
```

4. Ensure the certbot command is works

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

5. Run certbot command

```
sudo certbot --apache
```

6. Test auto renewal

```
sudo certbot renew --dry-run
```
