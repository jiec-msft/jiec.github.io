---
layout: post
title: How to log request processing time in JAX-RS filter
date: 2023-10-07 21:24:00 +0800
categories: JAX-RS
---
From: [jax rs - How to log request processing time in JAX-RS filter - Stack Overflow](https://stackoverflow.com/questions/61620936/how-to-log-request-processing-time-in-jax-rs-filter)

Instead you could save the start time on the properties of `ContainerRequestContext` on `ContainerRequestFilter.filter()` and then get it and use it on `ContainerResponseFilter.filter()`:

```Java
@Provider
public class RequestLogFilter implements ContainerRequestFilter, ContainerResponseFilter {

    @Override
    public void filter(ContainerRequestContext requestContext) {
        long requestStartTime = System.nanoTime();
        requestContext.setProperty("requestStartTime", requestStartTime);
    }

    @Override
    public void filter(ContainerRequestContext requestContext, ContainerResponseContext responseContext) {
        long requestStartTime = (long) requestContext.getProperty("requestStartTime");
        long requestFinishTime = System.nanoTime();
        long duration = requestFinishTime - requestStartTime;
        System.out.println("duration: " + TimeUnit.NANOSECONDS.toMillis(duration) + " ms");
    }
}
```