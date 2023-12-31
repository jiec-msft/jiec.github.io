---
layout: post
title:  "Meaning of eureka in Spring cloud eureka"
date:   2023-09-24 15:49:00 +0800
categories: Spring
---

Spring Cloud Eureka is named after the Greek word “Eureka” which means "I have found it!"1. The name “Eureka” was chosen because the service registry acts as a discovery mechanism for microservices1. It allows services to find and communicate with each other without hard-coding the hostname and port1. The service registry is the only ‘fixed point’ in such an architecture, with which each service has to register1. When a client registers with Eureka, it provides metadata about itself, such as host, port, health indicator URL, home page, and other details2. Eureka receives heartbeat messages from each instance belonging to a service2. If the heartbeat fails over a configurable timetable, the instance is normally removed from the registry2.


