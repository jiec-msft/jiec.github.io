---
layout: post
title:  "HTTP URL中放用户名密码的方法"
date:   2023-08-26 23:49:08 +0800
categories: HTTP
---

# 例子
http://username:password@domainname.com:port

# 服务器端收到了什么？
写个简单的spring boot web rest server
```java
package com.jiecmsft.webapp1;

import java.net.InetSocketAddress;
import java.util.List;
import java.util.Locale;
import java.util.Map;
import java.util.stream.Collectors;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.util.MultiValueMap;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ReadHeaderRestController {
    private static final Log LOG = LogFactory.getLog(ReadHeaderRestController.class);

    @GetMapping("/")
    public ResponseEntity<String> index() {
        return new ResponseEntity<String>("Index", HttpStatus.OK);
    }

    @GetMapping("/greeting")
    public ResponseEntity<String> greeting(@RequestHeader(value = HttpHeaders.ACCEPT_LANGUAGE) String language) {
        String greeting = "";
        List<Locale.LanguageRange> ranges = Locale.LanguageRange.parse(language);
        String firstLanguage = ranges.get(0).getRange();
        switch (firstLanguage) {
            case "es":
                greeting = "Hola!";
                break;
            case "de":
                greeting = "Hallo!";
                break;
            case "fr":
                greeting = "Bonjour!";
                break;
            case "en":
            default:
                greeting = "Hello!";
                break;
        }

        return new ResponseEntity<String>(greeting, HttpStatus.OK);
    }

    @GetMapping("/double")
    public ResponseEntity<String> doubleNumber(@RequestHeader("my-number") int myNumber) {
        return new ResponseEntity<String>(
                String.format("%d * 2 = %d", myNumber, (myNumber * 2)),
                HttpStatus.OK);
    }

    @GetMapping("/listHeaders")
    public ResponseEntity<String> listAllHeaders(@RequestHeader Map<String, String> headers) {

        StringBuilder allHeaders = new StringBuilder();
        headers.forEach((key, value) -> {
            LOG.info(String.format("Header '%s' = %s", key, value));
            allHeaders.append(String.format("'%s' = %s", key, value)).append(";");
        });

        return new ResponseEntity<String>(String.format("Listed %d headers, %s", headers.size(), allHeaders.toString()), HttpStatus.OK);
    }

    @GetMapping("/multiValue")
    public ResponseEntity<String> multiValue(@RequestHeader MultiValueMap<String, String> headers) {
        headers.forEach((key, value) -> {
            LOG.info(String.format("Header '%s' = %s", key, value.stream().collect(Collectors.joining("|"))));
        });

        return new ResponseEntity<String>(String.format("Listed %d headers", headers.size()), HttpStatus.OK);
    }

    @GetMapping("/getBaseUrl")
    public ResponseEntity<String> getBaseUrl(@RequestHeader HttpHeaders headers) {
        InetSocketAddress host = headers.getHost();
        String url = "http://" + host.getHostName() + ":" + host.getPort();
        return new ResponseEntity<String>(String.format("Base URL = %s", url), HttpStatus.OK);
    }

    @GetMapping("/nonRequiredHeader")
    public ResponseEntity<String> evaluateNonRequiredHeader(
            @RequestHeader(value = "optional-header", required = false) String optionalHeader) {

        return new ResponseEntity<String>(
                String.format("Was the optional header present? %s!", (optionalHeader == null ? "No" : "Yes")),
                HttpStatus.OK);
    }

    @GetMapping("/default")
    public ResponseEntity<String> evaluateDefaultHeaderValue(
            @RequestHeader(value = "optional-header", defaultValue = "3600") int optionalHeader) {

        return new ResponseEntity<String>(String.format("Optional Header is %d", optionalHeader), HttpStatus.OK);
    }
}
```

本地把web server跑起来，假设在默认端口8080.

- Client端直接call http://username:password@localhost:8080/listHeaders
- Response body: `Listed 7 headers, 'user-agent' = PostmanRuntime/7.32.3;'accept' = */*;'postman-token' = d09234ff-5c16-4685-81e8-b244617f5f95;'host' = localhost:8080;'accept-encoding' = gzip, deflate, br;'connection' = keep-alive;'authorization' = Basic dXNlcm5hbWU6cGFzc3dvcmQ=;`
- 然后
  - ```
    ➜  ~ echo -n "dXNlcm5hbWU6cGFzc3dvcmQ=" | base64 -d
    username:password
    ➜  ~
    ```
- 这里发现，其实，这个username:passward会被转成base64后放到authorization header里面，然后发到后端。


- Client端如果再指定这个authorization header: `curl --location 'http://username:password@localhost:8080/listHeaders' --header 'Authorization: Basic dXNlcm5hbWUyOnBhc3N3b3JkMg=='`
- Response body: `Listed 7 headers, 'authorization' = Basic dXNlcm5hbWUyOnBhc3N3b3JkMg==;'user-agent' = PostmanRuntime/7.32.3;'accept' = */*;'postman-token' = 40fdc62e-72d4-463a-868f-41f2c922a888;'host' = localhost:8080;'accept-encoding' = gzip, deflate, br;'connection' = keep-alive;`
- ```
  ➜  ~ echo -n "dXNlcm5hbWUyOnBhc3N3b3JkMg==" | base64 -d
  username2:password2
  ➜  ~
  ```

结果就是，会覆盖掉url中的cred。当然，谁会覆盖谁，这个可能取决于具体实现。但是应该总有一个被覆盖掉


