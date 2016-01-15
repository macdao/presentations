# Development based on Contract

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
┌─────┐     ┌─────────────┐
│ Web │ ──> │ Test Double │
└─────┘     └─────────────┘

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
                ╎ Mock Server   ╎
                ╎               ╎
┌─────┐  HTTP   ╎ ┌───────────┐ ╎
│ Web │ ──────> ╎ │ Contract  │ ╎
└─────┘         ╎ └───────────┘ ╎
                ╎               ╎
                └−−−−−−−−−−−−−−−┘
```


### Contract by Example



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
┌───────────────┐  Mock Server    ┌──────────┐  Test    ┌──────────────┐
│ Front-end dev │ <─────────────> │ Contract │ <──────> │ Back-end dev │
└───────────────┘                 └──────────┘          └──────────────┘
```



## There is c question


> Unit test, or API test, that is the question


- ProductController

```java
@RestController
@RequestMapping("/products")
public class ProductController {
    @RequestMapping(method = RequestMethod.GET)
    @PreAuthorize("hasRole('ADMIN')")
    public Page<Product> getAllProducts(@RequestParam(defaultValue = "0") int page, @RequestParam(defaultValue = "10") int size) {
        return productService.findAll(toPageable(page, size));
    }
}
```

- ProductService

```java
    public Page<Product> findAll(Pageable pageable) {
        return productRepository.findAll(pageable);
    }
```

- ProductRepository

```java
interface ProductRepository extends JpaRepository<Product, String> {
}
```


### Test Pyramid

<!-- .slide: data-background="white" -->

![Test Pyramid](contract-test/test-pyramid.png)



## API Blueprint


### Contract

```markdown
# Group Questions

## Question Collection [/questions]

### List All Questions [GET]

+ Response 200 (application/json)

        [
            {
                "question": "Favourite programming language?",
                "published_at": "2014-11-11T08:40:51.620Z",
                "url": "/questions/1",
                "choices": [
                    {
                        "choice": "Swift",
                        "url": "/questions/1/choices/1",
                        "votes": 2048
                    }, {
                        "choice": "Python",
                        "url": "/questions/1/choices/2",
                        "votes": 1024
                    }, {
                        "choice": "Objective-C",
                        "url": "/questions/1/choices/3",
                        "votes": 512
                    }, {
                        "choice": "Ruby",
                        "url": "/questions/1/choices/4",
                        "votes": 256
                    }
                ]
            }
        ]
```


### Mock Server 

```sh
drakov -f "contracts/*.md"
```


### Test

```sh
dredd "contracts/*.md" http://localhost:8080
```



## Moco


### Contract

```json
{
    "description": "anonymous_can_get_alligator",
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


### Mock Server

```sh
./moco http -p 12306 -g api.json
```


### Test

```java
public class AlligatorApiTest extends ApiTestBase {
    @Test
    public void anonymous_can_get_alligator() throws Exception {
        assertContract();
    }
}

```



## Moco / Mock Server


### Order

```json
[
    {
        "description": "anonymous_can_get_message_list_with_parameters",
        "request": {
            "uri": "/api/messages",
            "headers": {
                "Authorization": "Basic dXNlcjpwYXNzd29yZA=="
            },
            "queries": {
                "beginDate": "2015-10-01",
                "endDate": "2015-11-20"
            }
        },
        "response": {
            "status": 200,
            "json": {}
        }
    },
    {
        "description": "anonymous_can_get_message_list",
        "request": {
            "uri": "/api/messages",
            "headers": {
                "Authorization": "Basic dXNlcjpwYXNzd29yZA=="
            }
        },
        "response": {
            "status": 200,
            "json": {}
        }
    }
]
```


### File Upload 1

```json
{
    "description": "admin_can_upload_file",
    "request": {
        "uri": "/api/files",
        "method": "POST",
        "headers": {
            "Authorization": "Basic dXNlcjpwYXNzd29yZA=="
        }
    },
    "response": {
        "status": 201,
        "headers": {
            "Location": "http://localhost:{port}/api/files/{file-id}"
        }
    }
}
```


### api.json

```json
[
    {
        "include": "api/messages.json"
    },
    {
        "include": "api/files.json"
    }
]
```



## Moco / Test


### start up application

```java
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = Application.class)
@WebIntegrationTest("server.port:0")
public abstract class ApiTestBase {
    @Value("${local.server.port}")
    private int port;
}
```


### description

```json
[
    {
        "description": "admin_can_upload_file",
        "response": {
            "text": "foo"
        }
    }
]
```

```java
public class FileApiTest extends ApiTestBase {
    @Test
    public void admin_can_upload_file() throws Exception {
        assertContract();
    }
}
```


### cleaning

```java
    @Autowired
    private Flyway flyway;

    @Before
    public void setUp() throws Exception {
        flyway.clean();
        flyway.migrate();
    }
```


### File Upload 2

```json
  {
    "description": "admin_can_upload_file",
    "request": {
      "uri": "/api/files",
      "method": "POST",
      "headers": {
        "Authorization": "Basic eGlhb3N1bjpwYXNzd29yZA==",
        "Content-Type": "multipart/form-data; boundary=----WebKitFormBoundaryKgNrZfGWAQ59LuVd"
      },
      "file": "api/multipart.txt"
    },
    "response": {
      "status": 201,
      "headers": {
        "Location": "http://localhost:{port}/api/files/{file-id}"
      }
    }
  }
```

```
------WebKitFormBoundaryKgNrZfGWAQ59LuVd
Content-Disposition: form-data; name="file"; filename="hello.txt"
Content-Type: text/plain

hello
------WebKitFormBoundaryKgNrZfGWAQ59LuVd--
```


### Path Matcher

```java
    @Test
    public void admin_can_upload_file() throws IOException {
        final String fileId = assertContract().get("file-id");
    }

```


### Loose Match

```json
{
    "description": "anonymous_cannot_get_non_existent_message",
    "request": {
        "uri": "/api/messages/64"
    },
    "response": {
        "status": 404,
        "json": {
            "message": "根据相关法律法规和政策，部分搜索结果未予显示。"
        }
    }
}
```

```json
{
    "timestamp": "612889200000",
    "status": 404,
    "error": "Tiananmen Square protests of 1989",
    "exception": "NotFoundException",
    "message": "根据相关法律法规和政策，部分搜索结果未予显示。",
    "path": "/api/messages/64"
}
```



## beyond Moco

```json
[
    {
        "id": "product-id-1",
        "createdBy": {
            "id": "user-id-qixi",
            "username": "qixi",
            "authorities": [
                "ADMIN"
            ]
        }
    },
    {
        "id": "product-id-2",
        "createdBy": {
            "id": "user-id-qixi",
            "username": "qixi",
            "authorities": [
                "ADMIN"
            ]
        }
    }
]
```


```javascript
return [
    {
        "id": "product-id-1",
        "createdBy": users['user-id-qixi']
    },
    {
        "id": "product-id-2",
        "createdBy": users['user-id-qixi']
    }
];
```



## 参考资料

- <http://martinfowler.com/bliki/SpecificationByExample.html>
- <http://martinfowler.com/bliki/IntegrationContractTest.html>
- [消费者驱动契约测试](http://xichen2016.github.io/tutorial/2015/05/29/consumer-driven-contracts-test.html)
- [前后端分离了，然后呢？](http://icodeit.org/2015/06/whats-next-after-separate-frontend-and-backend/)
- [Swagger - 前后端分离后的契约](http://greengerong.com/blog/2015/07/29/swagger-qian-hou-duan-fen-chi-hou-de-qi-yue/)
- [Graph-Easy](https://github.com/ironcamel/Graph-Easy)
- [Pact](https://github.com/realestate-com-au/pact)
- [工作交流中怎样用一句话夹四五个英文单词来凸显逼格？](https://www.zhihu.com/question/24910138/answer/29412507)
- [API Blueprint](https://apiblueprint.org/)
