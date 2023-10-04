---
layout: post
title: A detailed comparison of REST and gRPC
date: 2023-10-04 20:30:00 +0800
categories: Tech
---
From [A detailed comparison of REST and gRPC | Kreya --- REST与gRPC详细对比 |克雷亚](https://kreya.app/blog/rest-vs-grpc/)
# A detailed comparison of REST and gRPC

April 26, 2023


For a long time, REST was the one and only "standard" for building APIs. It kind of replaced SOAP, which was an ugly mess of "too much XML". But in recent years, new alternatives have emerged. In 2015, Facebook released GraphQL to the public, and in 2016 Google followed suit with the release of [gRPC](https://grpc.io/). In this article, we are going to focus on the latter and compare it with REST, which is still widely used.  
长期以来，REST 是构建 API 的唯一“标准”。它在某种程度上取代了 SOAP，后者是“太多 XML”的丑陋混乱。但近年来，新的替代方案出现了。 2015 年，Facebook 向公众发布了 GraphQL，2016 年 Google 也紧随其后，发布了 gRPC。在本文中，我们将重点关注后者，并将其与仍然广泛使用的 REST 进行比较。

![REST vs gRPC thumbnail](https://kreya.app/thumbnails/rest-vs-grpc.png)

## Overview[​](https://kreya.app/blog/rest-vs-grpc/#overview "Direct link to Overview") 概述​

The following table will give you an overview of the discussed points and shows where REST and gRPC really shine.  
下表将概述所讨论的要点，并显示 REST 和 gRPC 的真正优势。

|Topic 话题|REST 休息|gRPC 远程过程调用|
|---|---|---|
|[Standardization  标准化](https://kreya.app/blog/rest-vs-grpc/#standardization)|No standard 无标准|Well defined 明确定义|
|[Paradigm  范例](https://kreya.app/blog/rest-vs-grpc/#fundamental-differences)|Resource based 基于资源|RPC|
|[Service modes  服务方式](https://kreya.app/blog/rest-vs-grpc/#service-modes)|Only unary 仅限一元|Unary, client streaming, server streaming and bidirectional streaming  <br>一元、客户端流、服务器流和双向流|
|[Requirements  要求](https://kreya.app/blog/rest-vs-grpc/#requirements)|Any HTTP version, JSON parser  <br>任何 HTTP 版本，JSON 解析器|HTTP/2, gRPC implementation for language  <br>HTTP/2、gRPC 语言实现|
|[API design  API设计](https://kreya.app/blog/rest-vs-grpc/#api-design)|Code first 代码优先|Design first 设计第一|
|[Default data format  默认数据格式](https://kreya.app/blog/rest-vs-grpc/#data-format)|JSON|Protobuf 原始缓冲区|
|[Web browser support  网络浏览器支持](https://kreya.app/blog/rest-vs-grpc/#browser-compatibility)|Native 本国的|gRPC web, via workarounds  <br>gRPC Web，通过解决方法|
|[Tools  工具](https://kreya.app/blog/rest-vs-grpc/#tooling)|More established tools 更成熟的工具|Language support varies, some with excellent implementations  <br>语言支持各不相同，有些具有出色的实现|

## Standardization[​](https://kreya.app/blog/rest-vs-grpc/#standardization "Direct link to Standardization") 标准化​

One of the disadvantages of REST is the lack of standardization. REST is more of a paradigm than an API standard and many folks mean different things when talking about it. For most, the term "REST API" is used for HTTP-based JSON APIs. For others, REST is used interchangeably with certain specifications such as [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) or [JSON:API](https://jsonapi.org/). But using XML instead of JSON would still make an API RESTful, even though that is not widely understood. The term REST is not even tied to HTTP. This can result in a lot of confusion when working with REST APIs. For example, consumers may automatically expect idempotency or cacheability of certain REST API endpoints, even though that is not defined explicitly.  
REST 的缺点之一是缺乏标准化。 REST 更像是一种范例，而不是 API 标准，许多人在谈论它时有不同的含义。对于大多数人来说，术语“REST API”用于表示基于 HTTP 的 JSON API。对于其他人来说，REST 可以与某些规范（例如 HATEOAS 或 JSON:API）互换使用。但是使用 XML 而不是 JSON 仍然可以使 API 成为 RESTful，尽管这一点尚未得到广泛理解。 REST 一词甚至与 HTTP 无关。在使用 REST API 时，这可能会导致很多混乱。例如，消费者可能会自动期望某些 REST API 端点的幂等性或可缓存性，即使没有明确定义。

In comparison, gRPC is well defined. For example, the [gRPC implementation over HTTP/2](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) is pretty detailed.  
相比之下，gRPC 定义明确。例如，通过 HTTP/2 的 gRPC 实现非常详细。

## Fundamental differences[​](https://kreya.app/blog/rest-vs-grpc/#fundamental-differences "Direct link to Fundamental differences")  
根本差异​

The paradigms of REST and gRPC are not the same.  
REST 和 gRPC 的范式并不相同。

With REST, everything centers around resources, which can be retrieved and manipulated. If we take a book as an example resource, a REST API will typically provide the following endpoints:  
通过 REST，一切都以资源为中心，可以检索和操作资源。如果我们以一本书作为示例资源，REST API 通常会提供以下端点：

- `GET /books` (fetch all books, most likely with parameters to filter and page the results)  
    `GET /books` （获取所有书籍，最有可能带有参数来过滤和分页结果）
- `GET /books/{id}` (fetch a specific book)  
    `GET /books/{id}` （获取特定书籍）
- `POST /books` (create a book)  
    `POST /books` （创建一本书）
- `DELETE /books/{id}` (delete a book)  
    `DELETE /books/{id}` （删除一本书）

and so on. Most HTTP-based REST APIs follow this pattern. While this works well, certain cases are difficult to represent as a REST API. For example, what if I want to create multiple books and do not want to call `POST /books` repeatedly for each book (for performance, idempotency or whatever reason)? Do I create a `POST /books/batch` endpoint? Is that still "RESTful"? While easy to solve technically, it often creates long discussions among developers.  
等等。大多数基于 HTTP 的 REST API 都遵循此模式。虽然这很有效，但某些情况很难表示为 REST API。例如，如果我想创建多本书并且不想为每本书重复调用 `POST /books` （出于性能、幂等性或任何原因）怎么办？我是否创建 `POST /books/batch` 端点？这还是“安逸”吗？虽然技术上很容易解决，但它经常会在开发人员之间引起长时间的讨论。

gRPC on the other hand is an RPC framework. It centers around service methods. If we take the book API example, with gRPC we would create a `BookService` with the following methods:  
另一方面，gRPC 是一个 RPC 框架。它以服务方法为中心。如果我们以书籍 API 为例，使用 gRPC，我们将使用以下方法创建一个 `BookService` ：

- `GetBooks()`
- `GetBook()`
- `CreateBook()`
- `DeleteBook()`

We can name these methods however we like and require whatever parameters we need. If we now want to add a method to create multiple books, nothing stops us from adding a `CreateBooks()` method. gRPC offers more "freedom" when designing APIs, as there are less (self-imposed) restrictions.  
我们可以根据自己的喜好命名这些方法，并要求我们需要的任何参数。如果我们现在想要添加一个方法来创建多本书，没有什么可以阻止我们添加 `CreateBooks()` 方法。 gRPC 在设计 API 时提供了更多“自由”，因为（自我施加的）限制更少。

### Service modes[​](https://kreya.app/blog/rest-vs-grpc/#service-modes "Direct link to Service modes") 服务模式​

gRPC supports four kind of service methods:  
gRPC支持四种服务方式：

- **Unary:** Send a single request, receive a single response  
    一元：发送单个请求，接收单个响应
- **Server streaming:** Send a single request, receive multiple responses  
    服务器流式传输：发送单个请求，接收多个响应
- **Client streaming:** Send multiple requests, receive a single response  
    客户端流：发送多个请求，接收单个响应
- **Bidirectional streaming:** Send multiple requests, receive multiple reponses  
    双向流：发送多个请求，接收多个响应

This is a very nice advantage of gRPC in comparison to REST, which only supports unary requests. Supporting other service modes in REST APIs would require the usage of a different protocol such as server-sent events or websockets, which isn't quite "RESTful".  
与仅支持一元请求的 REST 相比，这是 gRPC 的一个非常好的优势。在 REST API 中支持其他服务模式需要使用不同的协议，例如服务器发送的事件或 Websockets，这不太“RESTful”。

### Requirements[​](https://kreya.app/blog/rest-vs-grpc/#requirements "Direct link to Requirements") 要求​

REST APIs often "just work" with any kind of HTTP version. As long as a programming language has an HTTP client and a library for JSON parsing, consuming REST APIs is a breeze.  
REST API 通常适用于任何类型的 HTTP 版本。只要编程语言具有 HTTP 客户端和用于 JSON 解析的库，使用 REST API 就轻而易举。

gRPC explicitely needs HTTP/2 support, otherwise it will not work. In recent years, this has become less of a problem, as most proxies and frameworks have added support for HTTP/2.  
gRPC 明确需要 HTTP/2 支持，否则将无法工作。近年来，这已不再是一个问题，因为大多数代理和框架都增加了对 HTTP/2 的支持。

As gRPC requires code generation (for creating clients or server stubs), only [a set of programming languages](https://grpc.io/docs/languages/) are supported.  
由于 gRPC 需要生成代码（用于创建客户端或服务器存根），因此仅支持一组编程语言。

## API design[​](https://kreya.app/blog/rest-vs-grpc/#api-design "Direct link to API design") API设计​

A REST API is often the result of its implementation, dubbed "code-first." While it is possible to design the API with OpenAPI first and then generate a server stub, it is not an approach that many developers take. The OpenAPI definition is more likely generated from the API implementation, if there is an OpenAPI definition at all. As a result, the API definition is tightly coupled to the implementation. Changes in the wrong model/class can result in unintentional breaking changes of the API.  
REST API 通常是其实现的结果，被称为“代码优先”。虽然可以先使用 OpenAPI 设计 API，然后生成服务器存根，但这并不是许多开发人员采用的方法。如果存在 OpenAPI 定义，则 OpenAPI 定义更有可能是从 API 实现生成的。因此，API 定义与实现紧密耦合。错误模型/类的更改可能会导致 API 意外发生重大更改。

gRPC uses a different approach, where the API has to be defined before one can implement it (dubbed "design-first"). Clients and server stubs are then generated from this API definition. This requires some thinking ahead, as one cannot jump directly into implementing the API.  
gRPC 使用不同的方法，必须先定义 API，然后才能实现它（称为“设计优先”）。然后根据此 API 定义生成客户端和服务器存根。这需要提前进行一些思考，因为无法直接跳到实现 API。

Both approaches have their pros and cons. The usual REST API approach allows quicker iteration, as the server is always the source of truth. With gRPC, it can be annoying to first change the API definition before being able to adjust the implementation. However, it brings some safety benefits by having the API explicitly defined.  
两种方法都有其优点和缺点。通常的 REST API 方法允许更快的迭代，因为服务器始终是事实的来源。对于 gRPC，在调整实现之前先更改 API 定义可能会很烦人。然而，它通过显式定义 API 带来了一些安全优势。

## Data format[​](https://kreya.app/blog/rest-vs-grpc/#data-format "Direct link to Data format") 数据格式​

Both REST and gRPC can use different formats to transfer data. Most REST APIs use JSON, while gRPC uses [Protocol Buffers](https://protobuf.dev/) (Protobuf) by default, so we will compare these two.  
REST 和 gRPC 都可以使用不同的格式来传输数据。大多数 REST API 使用 JSON，而 gRPC 默认使用 Protocol Buffers (Protobuf)，因此我们将比较这两者。

JSON has limited support for data types and also has some quirks (ex. big numbers need to be represented as strings). It is a text format and human readable. Field names are serialized, which takes up some space. In some programming languages, this also requires the use of reflection to deserialize JSON messages, which is quite slow.  
JSON 对数据类型的支持有限，并且也有一些怪癖（例如，大数字需要表示为字符串）。它是一种人类可读的文本格式。字段名称是序列化的，会占用一些空间。在某些编程语言中，这还需要使用反射来反序列化 JSON 消息，这是相当慢的。

As written above, gRPC APIs and the respective message types are first defined as Protocol Buffers. Each message is strongly typed, may contain useful comments and has lots of other interesting features. For the supported list of programming languages, code to (de-)serialize messages can be automatically generated. As it is a binary format and does not serialize the field names, it uses less space than an equivalent JSON message. This does have the drawback that it is no longer human readable and requires the Protobuf definition to deserialize messages, which can hamper the developer experience.  
如上所述，gRPC API 和相应的消息类型首先被定义为协议缓冲区。每条消息都是强类型的，可能包含有用的注释，并且有许多其他有趣的功能。对于受支持的编程语言列表，可以自动生成用于（反）序列化消息的代码。由于它是二进制格式并且不会序列化字段名称，因此它比等效的 JSON 消息使用更少的空间。这样做的缺点是它不再是人类可读的，并且需要 Protobuf 定义来反序列化消息，这可能会妨碍开发人员的体验。

The following JSON example would take up roughly 66 bytes (with whitespace removed).  
以下 JSON 示例将占用大约 66 个字节（删除空格）。

```
{  "persons": [    {      "name": "Max",      "age": 23    },    {      "name": "Mike",      "age": 52    }  ]}
```

An equivalent serialized protobuf message would only use 19 bytes.  
等效的序列化 protobuf 消息仅使用 19 个字节。

```
0x0A070A034D617810170A080A0448616E731034
```

### Large messages[​](https://kreya.app/blog/rest-vs-grpc/#large-messages "Direct link to Large messages") 大消息​

Protobuf is designed to serialize and deserialize messages in-memory. As a result, transferring huge messages with Protobuf/gRPC is not recommended. Most gRPC implementation place a default limit of 4 MB on individual messages.  
Protobuf 旨在对内存中的消息进行序列化和反序列化。因此，不建议使用 Protobuf/gRPC 传输大消息。大多数 gRPC 实现对单个消息设置默认的 4 MB 限制。

Handling large data sizes with REST APIs, such as file uploads, is rather straight forward. The received file can be treated as a stream, using very little memory. This is not impossible with gRPC, but needs more manual effort. The file would have to be chunked into several parts on the sender side. Each chunk would then be sent as an individual message via a client-streaming method to the server. The server receives each chunk and can construct a data stream from it, resulting in a similar behaviour to the REST API, although with more effort.  
使用 REST API 处理大数据（例如文件上传）相当简单。接收到的文件可以被视为流，使用很少的内存。这对于 gRPC 来说并非不可能，但需要更多的手动工作。在发送方，文件必须被分成几个部分。然后，每个块将作为单独的消息通过客户端流方法发送到服务器。服务器接收每个块并可以从中构建数据流，从而产生与 REST API 类似的行为，尽管需要付出更多努力。

## Browser compatibility[​](https://kreya.app/blog/rest-vs-grpc/#browser-compatibility "Direct link to Browser compatibility") 浏览器兼容性​

This is where REST really shines. It is supported natively by web browsers, making it effortless to consume REST APIs from web applications.  
这就是 REST 真正发挥作用的地方。 Web 浏览器原生支持它，因此可以轻松地从 Web 应用程序使用 REST API。

gRPC is not directly supported by browsers, as it requires explicit HTTP/2 support and access to certain HTTP/2 features, which web browsers do not provide. As a workaround, [gRPC Web](https://github.com/grpc/grpc-web) can be used. It is a slight variation of the gRPC protocol to make it consumable by web browsers. For some programming languages, gRPC Web support is already included in the framework. For others, a proxy is needed to translate gRPC traffic into gRPC Web traffic and vice versa. In comparison to REST APIs, which require no dependencies, gRPC APIs are more cumbersome to consume from the web.  
gRPC 不受浏览器直接支持，因为它需要显式 HTTP/2 支持并访问某些 HTTP/2 功能，而 Web 浏览器不提供这些功能。作为解决方法，可以使用 gRPC Web。它是 gRPC 协议的一个细微变化，使其可供 Web 浏览器使用。对于某些编程语言，gRPC Web 支持已包含在框架中。对于其他人，需要代理将 gRPC 流量转换为 gRPC Web 流量，反之亦然。与不需要依赖项的 REST API 相比，gRPC API 在网络上使用起来更加麻烦。

A workaround could be to use [JSON transcoding](https://cloud.google.com/endpoints/docs/grpc/transcoding), which allows developers to expose a gRPC API as a REST API.  
解决方法可能是使用 JSON 转码，它允许开发人员将 gRPC API 公开为 REST API。

## Tooling[​](https://kreya.app/blog/rest-vs-grpc/#tooling "Direct link to Tooling") 工装​

gRPC and REST tooling varies heavily between programming languages and frameworks. In some, gRPC feels more "native" while in others, the REST tooling is much more advanced.  
gRPC 和 REST 工具在编程语言和框架之间存在很大差异。在某些方面，gRPC 感觉更“原生”，而在另一些方面，REST 工具则要先进得多。

Proper language support for gRPC matters a lot more, since it requires tooling to generate clients and server stubs. For non-supported programming, you are out of luck. Clients for REST APIs can always be created manually, but this may take some effort. While tools to create REST clients from OpenAPI definitions exist, their developer experience is often lackluster in comparison to the gRPC equivalent.  
对 gRPC 的适当语言支持更为重要，因为它需要工具来生成客户端和服务器存根。对于不受支持的编程，你就不走运了。 REST API 的客户端始终可以手动创建，但这可能需要一些努力。虽然存在根据 OpenAPI 定义创建 REST 客户端的工具，但与 gRPC 等价物相比，它们的开发人员体验通常乏善可陈。

Since REST APIs have been around for much longer, more tools that help build, test and deploy REST APIs exist. Their functionality is often more advanced than that of gRPC tools. This is also one of the main reasons why we built [Kreya](https://kreya.app/), which tries to be the best gRPC GUI client (while also supporting REST).  
由于 REST API 已经存在了很长时间，因此存在更多有助于构建、测试和部署 REST API 的工具。它们的功能通常比 gRPC 工具更先进。这也是我们构建 Kreya 的主要原因之一，它试图成为最好的 gRPC GUI 客户端（同时也支持 REST）。

## Conclusion[​](https://kreya.app/blog/rest-vs-grpc/#conclusion "Direct link to Conclusion") 结论​

REST and gRPC both have their advantages and disadvantages.  
REST 和 gRPC 都有各自的优点和缺点。

Consuming REST APIs from web apps is generally easier. REST is also more widely used, making it simpler to use for some developers, as they may not know about gRPC.  
从 Web 应用程序使用 REST API 通常更容易。 REST 也得到了更广泛的使用，这使得一些开发人员更容易使用，因为他们可能不了解 gRPC。

In my opinion, gRPC definitely has advantages in server-to-server communications (ex. between microservices). The ability to share the exact API definition and to create API clients in multiple programming languages is a huge win.  
在我看来，gRPC 在服务器到服务器通信（例如微服务之间）方面绝对具有优势。能够共享准确的 API 定义并以多种编程语言创建 API 客户端是一个巨大的胜利。

There is no definitive answer to "Should we use REST or gRPC?". Some APIs could have unique use cases for which gRPC or REST may prove a better fit. Or developers could simply be more comfortable or experienced with either REST or gRPC. All these reasons are fine. In the end, everyone has to decide for themselves which technology to use.  
“我们应该使用 REST 还是 gRPC？”没有明确的答案。某些 API 可能有独特的用例，gRPC 或 REST 可能更适合。或者，开发人员可能对 REST 或 gRPC 更加熟悉或更有经验。所有这些理由都​​很好。最后，每个人都必须自己决定使用哪种技术。
