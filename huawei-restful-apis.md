# Homework Review



# RESTful APIs



## [Google Contacts API version 3.0](https://developers.google.com/google-apps/contacts/v3/)

### Retrieving all contacts

- Request

```
GET /m8/feeds/contacts/default/full
```
- Response

```
HTTP/1.1 200 OK
Content-Type: application/atom+xml; charset=UTF-8; type=feed
...

<feed>
  <id>userEmail</id>
  <updated>2008-12-10T10:04:15.446Z</updated>
  <entry>
    <id>
      http://www.google.com/m8/feeds/contacts/userEmail/base/contactId
    </id>
  </entry>
  <!-- Other entries ... -->
</feed>
```


### Retrieving contacts using query parameters

`GET https://www.google.com/m8/feeds/contacts/{userEmail}/full?updated-min=2007-03-16T00:00:00`


### Retrieving a single contact

- Request

```
GET /m8/feeds/contacts/default/full/{contactId}
```
- Response

```
HTTP/1.1 200 OK
Content-Type: application/atom+xml; charset=UTF-8; type=feed
...

<entry>
  <id>
    http://www.google.com/m8/feeds/contacts/{userEmail}/base/{contactId}
  </id>
</entry>
```


### Creating contacts

- Request

```
POST /m8/feeds/contacts/default/full
...
<atom:entry>
  <atom:category scheme='http://schemas.google.com/g/2005#kind'
    term='http://schemas.google.com/contact/2008#contact'/>
  <gd:name>
     <gd:givenName>Elizabeth</gd:givenName>
     <gd:familyName>Bennet</gd:familyName>
     <gd:fullName>Elizabeth Bennet</gd:fullName>
  </gd:name>
  <atom:content type='text'>Notes</atom:content>
  <gd:email rel='http://schemas.google.com/g/2005#work'
    primary='true'
    address='liz@gmail.com' displayName='E. Bennet'/>
</atom:entry>
```
- Response

```
HTTP/1.1 201 Created
Content-Type: application/atom+xml; charset=UTF-8; type=feed
...
<atom:entry>
  <id>http://www.google.com/m8/feeds/contacts/userEmail</bar>/base/{contactId}</id>
  <updated>2008-12-10T04:45:03.331Z</updated>
  <title>Elizabeth Bennet</title>
  <gd:name>
     <gd:givenName>Elizabeth</gd:givenName>
     <gd:familyName>Bennet</gd:familyName>
     <gd:fullName>Elizabeth Bennet</gd:fullName>
  </gd:name>
</atom:entry>
```

### Deleting contacts

- Request

```
DELETE /m8/feeds/contacts/default/full/contactId
If-match: Etag
```
- Response

```
HTTP/1.1 200 OK
```



## Representational State Transfer (REST)

- is a software architecture style
- consisting of guidelines and best practices for creating scalable web services.
- "Architectural Styles and the Design of Network-based Software Architectures"


### Roy Thomas Fielding

![Roy Thomas Fielding](huawei-restful-apis/roy_fielding.jpg)

- principal authors of the HTTP specification
- co-founder of the Apache HTTP Server project
- the chair of the Apache Software Foundation for its first three years


> 在为HTTP/1.1和URI的新标准设计扩展时，我认识到需要建立一个关于万维网应该如何运转的模型。这个关于整个Web应用中的交互的理想化的模型被称作表述性状态转移(REST)架构风格，成为了现代Web架构的基础。


> 这个名称“表述性状态转移”是有意唤起人们对于一个良好设计的Web应用如何运转的印象：一个由网页组成的网络（一个虚拟状态机），用户通过选择链接（状态转移）在应用中前进，导致下一个页面（代表应用的下一个状态）被转移给用户，并且呈现给他们，以便他们来使用。


## REST架构约束

- Client-Server
- Stateless
- Cache
- Uniform Interface
- Layered System
- Code-On-Demand (optional)

REST架构过程视图

![REST Process View](huawei-restful-apis/rest_process_view.gif)


按照Fielding的描述，REST的统一接口由4个部分组成：资源的标识、通过表述对资源执行的操作、自描述的消息、以及作为应用状态引擎的超媒体（现在通常缩写为HATEOAS）。以HTTP为例，资源的标识就是资源的URI；资源的表述是资源在特定时刻状态的描述，可以通过在客户-服务器之间传递资源的表述，对资源执行某种操作；自描述的消息由一些标准的HTTP方法、可定制的HTTP头信息、可定制的HTTP响应代码组成；超媒体就是HTML，可以使用HTML作为引擎，驱动应用状态的迁移。


### 资源

- REST对于信息的核心抽象是资源。
- 任何能够被命名的信息都能够作为一个资源：
 - 一份文档或
 - 一张图片
 - 一个与时间相关的服务(比如洛杉矶今日的天气)
 - 一个其他资源的集合
 - 一个非虚拟的对象(比如一个人)等等。


 - 一篇学术论文的创作者首选的版本
 - X会议学报中发表的论文
 - 最新版本
 - 版本号 1.2.7
 - 包含有 Orange 功能实现的修订版本


REST使用一个资源标识符来标识特定资源


### 表述

- REST组件通过以下方式在一个资源上执行动作：使用一个表述来捕获资源的当前的或预期的状态，并在组件之间传递该表述。
- 表述的其他常用但不够精确的名称包括：文档、文件、HTTP消息实体、实例或变量。
- 表述由数据、描述数据的元数据、以及（有时候存在的）描述元数据的元数据组成（通常用来验证消息的完整性）。


### 内容协商

REST组件通过以一种表述来进行通信，可以基于接收者的能力或者其期待的内容、以及资源的性质来动态地选择不同的表述


### RPC

连接器接口与过程调用有些类似,但是在参数和结果的传递方式上有着重要的区别。其传入参数由请求的控制数据、一个表示请求的目标的资源标识符、以及一个可选的表述组成。其传出参数由响应的控制数据、可选的资源元数据、以及一个可选的表述组成。从一种抽象的观点来看,调用是同步的,但是传入参数和传出参数都可以作为数据流来传递。换句话说,处理可以在完全知道参数的值(译者注:即数据流的全部数据)之前进行,从而避免了对于大量数据转移进行批量处理而产生的延迟。


## RESTful APIs

- 遵守REST架构约束的Web service APIs被称作RESTful APIs



# 课后练习




# 参考资料

- [Representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)
- [Architectural Styles and the Design of Network-based Software Architectures](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
- [介绍Web基础架构设计原则的经典论文《架构风格与基于网络的软件架构设计》导读](http://www.infoq.com/cn/articles/doctor-fielding-article-review)