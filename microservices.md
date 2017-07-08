# 微服务

`https://martinfowler.com/articles/microservices.html`



## 微服务架构的九大特性


### 单块应用和微服务

![单块应用和微服务](https://martinfowler.com/articles/microservices/images/sketch.png)

Note:

- 一个单块应用三个主要部分：客户端用户界面、数据库和服务端应用系统
- 应用系统的一个很小的部分的一处变更，也需要将整个单块应用系统进行重新构建和部署
- 随着时间的推移，单块应用开始变得经常难以保持一个良好的模块化结构，这使得它变得越来越难以将一个模块的变更的影响控制在该模块内


### 特性一：“组件化”与“多服务”

Note:

- 可被独立部署
- 远程调用更加昂贵。所以远程调用API接口必须是粗粒度的


### 特性二：围绕“业务功能”组织团队


> 任何设计（广义上的）系统的组织，都会产生这样一个设计，即该设计的结构与该组织的沟通结构相一致。
>
>——梅尔文•康威（Melvyn Conway）, 1967年


#### 康威定律在起作用

![](https://martinfowler.com/articles/microservices/images/conways-law.png)


#### 被团队边界所强化的服务边界

![](https://martinfowler.com/articles/microservices/images/PreferFunctionalStaffOrganization.png)


### 特性三：“做产品”而不是“做项目”

> 谁构建，谁运行

Note:

- 单块应用系统的开发工作也可以
- 这样的“产品”理念，是与业务功能的联动绑定在一起的


### 特性四：“智能端点”与“傻瓜管道”

Note:

- ESB产品经常包括高度智能的设施，来进行消息的路由、编制(choreography)、转换，并应用业务规则。


> 成为Web，而不是躲着Web (Be of the web, not behind the web)
>
> ——Ian Robinson

Note:

- 轻量级的消息总线
- 将一个单块系统改造为若干微服务的最大问题，在于对通信模式的改变


### 特性五：“去中心化”地治理技术

Note:

- 根据工作的不同来选用合理的工具


### 特性六：“去中心化”地管理数据

![](https://martinfowler.com/articles/microservices/images/decentralised-data.png)

Note: 

- 限界上下文
- 多语种持久化
- 最终达到一致


### 特性七：“基础设施”自动化

![](https://martinfowler.com/articles/microservices/images/basic-pipeline.png)

Note:

- AWS
- CI
- CD
- 自动化测试
- 自动化部署


让“部署”工作变得“无聊”


### 特性八：“容错”设计

Note:

- 与一个单块设计相比，这是一个劣势
- 监控


### 特性九：“演进式”设计

Note:

- 控制应用系统中的变化，而无须减少变化的发生
- 许多服务将来会报废，而不是守着这些服务做长期演进
- “一旦赛事结束，这样页面就可以被删除”
- 变化模式：
 - 将那些能在同时发生变化的东西，放到同一个模块中
 - 系统中那些很少发生变化的部分，应该被放到不同的服务中，以区别于那些当前正在经历大量变动的部分
 - 如果发现需要同时反复变更两个服务时，这就是它们两个需要被合并的一个信号
- 版本化



## 未来的方向是“微服务”吗？


### 难点

- 搞清楚某个组件的边界
- 将组件内的复杂性转移到组件之间的连接
- 技术更加过硬的团队


### 不要一上来就以微服务架构做为起点

- 要用一个单块系统做为起点，并保持其模块化。
- 当这个单块系统出现了问题后，再将其分解为微服务。


### 谨慎乐观

到目前为止，我们已经看到足够多的有关微服务风格的事物，并且觉得这是一条有价值去跋涉的道路。我们不能肯定地说，道路的尽头在哪里。但是，软件开发的挑战之一，就是只能基于“目前手上拥有但还不够完善”的信息来做出决策。



## Microservice Premium

Note:

- automated deployment
- monitoring
- dealing with failure
- eventual consistency
- and other factors that a distributed system introduces


![](https://martinfowler.com/bliki/images/microservice-verdict/productivity.png)



## Microservice Prerequisites


![](https://martinfowler.com/bliki/images/microservicePrerequisites/sketch.png)


- Rapid provisioning
- Basic Monitoring
- Rapid application deployment
- DevOps culture



## 参考资料

- https://martinfowler.com/articles/microservices.html
- https://martinfowler.com/bliki/MicroservicePremium.html
- https://martinfowler.com/bliki/MicroservicePrerequisites.html
- https://martinfowler.com/microservices/