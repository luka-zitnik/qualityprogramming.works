---
layout: post
title: Debugging Cross-Domain Requests With Xdebug
subtitle: Overcoming Limitations of Xdebug Browser Extensions
categories: [quick tip]
date: 2019-02-02 00:00 +0000
background: /images/alexandre-godreau-203580-unsplash.jpg
---

You may have noticed that your browser extension does not suffice to debug cross-domain requests. Your ajax requests do not contain the `XDEBUG_SESSION` cookie. You've got these two options.

## Option #1: With Credentials

You can set a matching configuration, `withCredentials: true` on the request side and `Control-Allow-Credentials: true` on the response side. The former forces the browser to send cookies, even from a different domain and the latter tells the browser to accept the response to such request.

## Option #2: Modifying Requests On-The-Fly

Another option is to intercept the requests and add `XDEBUG_SESSION` cookie or `XDEBUG_SESSION_START` query parameter. You can modify requests using another browser extension, like the *Requestly* for Chrome.
