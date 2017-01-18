# Contract by example

Qi Xi / Macdao

from ThoughtWorks



## The problem


### Seperating frontend and backend

<table>
<tr>
<td width="500px">
<pre>
```
┌−−−−−−−−−−−−−−−┐
╎ Frontend      ╎
╎               ╎
╎ ┌───────────┐ ╎
╎ │ AngularJS │ ╎
╎ └───────────┘ ╎
╎ ┌───────────┐ ╎
╎ │    CSS    │ ╎
╎ └───────────┘ ╎
╎ ┌───────────┐ ╎
╎ │   HTML5   │ ╎
╎ └───────────┘ ╎
╎ ┌───────────┐ ╎
╎ │  Node.js  │ ╎
╎ └───────────┘ ╎
╎ ┌───────────┐ ╎
╎ │   React   │ ╎
╎ └───────────┘ ╎
╎ ┌───────────┐ ╎
╎ │  Vue.js   │ ╎
╎ └───────────┘ ╎
╎ ┌───────────┐ ╎
╎ │  Webpack  │ ╎
╎ └───────────┘ ╎
╎               ╎
└−−−−−−−−−−−−−−−┘
```
</pre>
</td>
<td width="500px">
<pre>
```
┌−−−−−−−−−−−−−−−−−−−−−┐
╎ Backend             ╎
╎                     ╎
╎ ┌─────────────────┐ ╎
╎ │     Flyway      │ ╎
╎ └─────────────────┘ ╎
╎ ┌─────────────────┐ ╎
╎ │     Gradle      │ ╎
╎ └─────────────────┘ ╎
╎ ┌─────────────────┐ ╎
╎ │      Maven      │ ╎
╎ └─────────────────┘ ╎
╎ ┌─────────────────┐ ╎
╎ │      MySQL      │ ╎
╎ └─────────────────┘ ╎
╎ ┌─────────────────┐ ╎
╎ │   Spring Boot   │ ╎
╎ └─────────────────┘ ╎
╎ ┌─────────────────┐ ╎
╎ │   Spring MVC    │ ╎
╎ └─────────────────┘ ╎
╎ ┌─────────────────┐ ╎
╎ │ Spring Security │ ╎
╎ └─────────────────┘ ╎
╎                     ╎
└−−−−−−−−−−−−−−−−−−−−−┘
```
</pre>
</td>
</tr>
</table>


```
┌──────────────────────┐  Ajax   ┌────────────────────────┐
│ Browser ( Frontend ) │ ──────> │ RESTful APIs (Backend) │
└──────────────────────┘         └────────────────────────┘
```

Note: SEO?


### Requirements


#### 1. Contracts


#### 2. Mock server for frontend

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


#### 3. Verify backend

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


#### 4. Changes should break build



## Consumer-Driven Contracts
### A Service Evolution Pattern


### pact

![pact](https://raw.githubusercontent.com/realestate-com-au/pact/master/documentation/pact_two_parts.png)


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


### Requirements

1. Contracts
2. Mock server for frontend
3. Verify backend
4. Changes should break build


```
┌──────────┐     ┌───────────┐     ┌─────────┐
│ Frontend │ ──> │ Contracts │ ··> │ Backend │
└──────────┘     └───────────┘     └─────────┘

```


<!-- .slide: data-background="white" -->
![Bounded Context](https://martinfowler.com/bliki/images/boundedContext/sketch.png)


```
┌─────┐  contracts   ┌─────┐  contracts   ┌──────────────┐
│ Web │ ───────────> │ BFF │ ───────────> │ RESTful APIs │
└─────┘              └─────┘              └──────────────┘
```



## Provider Contracts


### 1. Contracts

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


### 2. Mock server for frontend

![Moco](https://raw.githubusercontent.com/dreamhead/moco/master/moco-doc/DukeChoice-960x90-lm.png)

```sh
./moco http -p 12306 -g api.json
```


### moscow

```java
public class AlligatorApiTest extends ApiTestBase {
    @Test
    public void anonymous_can_get_alligator() throws Exception {
        assertContract();
    }
}
```


- raml
- swagger http://greengerong.com/blog/2015/07/29/swagger-qian-hou-duan-fen-chi-hou-de-qi-yue/
- API Blueprint



## Specification By Example


```sql
SELECT
    [ALL | DISTINCT | DISTINCTROW ]
      [HIGH_PRIORITY]
      [MAX_STATEMENT_TIME = N]
      [STRAIGHT_JOIN]
      [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
      [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr ...]
    [FROM table_references
      [PARTITION partition_list]
    [WHERE where_condition]
    [GROUP BY {col_name | expr | position}
      [ASC | DESC], ... [WITH ROLLUP]]
    [HAVING where_condition]
    [ORDER BY {col_name | expr | position}
      [ASC | DESC], ...]
    [LIMIT {[offset,] row_count | row_count OFFSET offset}]
    [PROCEDURE procedure_name(argument_list)]
    [INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        export_options
      | INTO DUMPFILE 'file_name'
      | INTO var_name [, var_name]]
    [FOR UPDATE | LOCK IN SHARE MODE]]
```


```sql
SELECT 1 + 1;
```


One of the great things about specification by example is that examples are usually much easier to come up with, particularly for the non-nerds who we write the software for.

-- Martin Fowler



## References

- http://martinfowler.com/bliki/SpecificationByExample.html
- http://martinfowler.com/bliki/IntegrationContractTest.html
- https://www.martinfowler.com/articles/consumerDrivenContracts.html
- https://github.com/macdao/moscow
- https://github.com/realestate-com-au/pact