# Development based on Contracts

## from project `China Merchants Bank - APP Development`



## Context


### Front-end

- coral
- HTML5
- AngularJS 1.4
- Sass
- restify(coral mock)


### Back-end

- Java 7
- Spring Boot
- JBoss
- Flyway
- Gradle
- MySQL
- Joda-Time


### Deployment

```
  ┌─────────────┐
  │   Browser   │
  └─────────────┘
    │
    │
    ∨
┌−−−−−−−−−−−−−−−−−−−−−−−−−−−−┐
╎ Server                     ╎
╎                            ╎
╎ ┌────────────────────────┐ ╎
╎ │     Reverse Proxy      │ ╎
╎ │        (nginx)         │ ╎
╎ └────────────────────────┘ ╎
╎   │              │         ╎
╎   │              │         ╎
╎   ∨              ∨         ╎
╎ ┌─────────────┐┌─────────┐ ╎
╎ │ RESTful API ││   Web   │ ╎
╎ │   (JBoss)   ││ (nginx) │ ╎
╎ └─────────────┘└─────────┘ ╎
╎                            ╎
└−−−−−−−−−−−−−−−−−−−−−−−−−−−−┘

```


### Process

```
┌─────┐  HTTP   ┌─────────────┐
│ Web │ ──────> │ RESTful API │
└─────┘         └─────────────┘
```


### Integration Contract Test

<!-- .slide: data-background="white" -->

![IntegrationContractTest](contract-test/IntegrationContractTest.png)


### Test Double

```
┌─────┐  HTTP   ┌─────────────┐
│ Web │ ──────> │ Test Double │
└─────┘         └─────────────┘

```



## There is a question


### `diff test-double contract`


```ruby
    before do
      animal_service.given("an alligator exists").
        upon_receiving("a request for an alligator").
        with(method: :get, path: '/alligator', query: '').
        will_respond_with(
          status: 200,
          headers: {'Content-Type' => 'application/json'},
          body: {name: 'Betty'} )
    end
```
vs.

```json
{
    "request": {
        "method": "GET",
        "uri": "/alligator"
    },
    "response": {
        "status": 200,
        "headers": {
            "content-type": "application/json"
        },
        "json": {
            "name": "Betty"
        }
    }
}
```


```
                ┌−−−−−−−−−−−−−−−┐
                ╎ Test Double   ╎
                ╎               ╎
┌─────┐  HTTP   ╎ ┌───────────┐ ╎
│ Web │ ──────> ╎ │ Contract  │ ╎
└─────┘         ╎ └───────────┘ ╎
                ╎               ╎
                └−−−−−−−−−−−−−−−┘
```



## There is b question


### `a_contract_change_should_fail_tests`


```
┌──────────┐     ┌─────────────┐
│ Contract │     │ RESTful API │
└──────────┘     └─────────────┘
```


```
┌−−−−−−−−−−−−−−┐
╎ Test         ╎
╎              ╎
╎ ┌──────────┐ ╎  HTTP   ┌─────────────┐
╎ │ Contract │ ╎ ──────> │ RESTful API │
╎ └──────────┘ ╎         └─────────────┘
╎              ╎
└−−−−−−−−−−−−−−┘

```


### Collaboration

```
┌───────────────┐      ┌──────────┐      ┌──────────────┐
│ Front-end dev │ <──> │ Contract │ <──> │ Back-end dev │
└───────────────┘      └──────────┘      └──────────────┘
```

pact

blueprint

测试数据准备

path matcher

partical match

file upload


## beyond Moco



## 参考资料

- <http://martinfowler.com/bliki/SpecificationByExample.html>
- <http://martinfowler.com/bliki/IntegrationContractTest.html>
- [消费者驱动契约测试](http://xichen2016.github.io/tutorial/2015/05/29/consumer-driven-contracts-test.html)
- [前后端分离了，然后呢？](http://icodeit.org/2015/06/whats-next-after-separate-frontend-and-backend/)
- [Swagger - 前后端分离后的契约](http://greengerong.com/blog/2015/07/29/swagger-qian-hou-duan-fen-chi-hou-de-qi-yue/)
- [Graph-Easy](https://github.com/ironcamel/Graph-Easy)
- [Pact](https://github.com/realestate-com-au/pact)
- [工作交流中怎样用一句话夹四五个英文单词来凸显逼格？](https://www.zhihu.com/question/24910138/answer/29412507)