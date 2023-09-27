---
layout: post
title:  "Nginx 499 status code"
date:   2023-09-27 15:49:00 +0800
categories: Spring
---

HTTP 499 in Nginx means that the client closed the connection before the server answered the request. In my experience is usually caused by client side timeout. As I know it's an Nginx specific error code.

Learn more [Possible reason for NGINX 499 error codes](https://stackoverflow.com/questions/12973304/possible-reason-for-nginx-499-error-codes)