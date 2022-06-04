---
sidebar_position: 7
---

# HA Proxy

We will redirect multiple domains to multiple local servers using SNI

In this guide we will configure multiple domains (`example1.com`, `example2.com`) to redirect to specific local server (`10.10.10.254`, `10.10.10.230`) using the same port (`HTTPS/443`)

For this example our local servers are using reverse proxies and each server issue it's own certs using let's encrypt, and only accepting HTTPS traffic.
(Let's encrypt uses DNS validation (Cloudflare) so we don't need `HTTP/80` port open)