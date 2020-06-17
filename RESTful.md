# RESTful

### RESTful概述

RESTful是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用。

REST是`Re`presentational `S`tate `T`ransfer的缩写，是Roy Thomas Fielding在他2000年的博士论文中提出的。其提出的设计概念和准则为：

1. 网络上的所有事物都可以抽象为资源
2. 每个资源都应该有唯一的标识（identifier），对资源的操作不会改变标识
3. 所有的操作都是无状态的
4. 使用标准方法（GET、POST、PUT、PATCH、DELETE）操作资源

## RESTful使用

| HTTP方法 | URI | 描述 | 幂等 | 安全 |
| -------- | --- | --- | --- | --- |
| GET | /api/members | 获取成员列表 | 是 | 是 |
| GET | /api/members/{id} | 获取指定成员 | 是 | 是 |
| POST | /api/members | 创建一个成员 | 否 | 否 |
| PUT | /api/members/{id} | 更新成员所有信息 | 是 | 否 |
| PATCH | /api/members/{id} | 更新成员部分信息 | 是 | 否 |
| DELETE | /api/members/{id} | 删除指定成员 | 是 | 否 |

| HTTP方法 | URI | 描述 | 幂等 | 安全 |
| -------- | --- | --- | --- | --- |
| GET | /api/groups | 获取群组列表 | 是 | 是 |
| GET | /api/groups/{id} | 获取指定群组 | 是 | 是 |
| POST | /api/groups | 创建一个群组 | 否 | 否 |
| PUT | /api/groups/{id} | 更新群组所有信息 | 是 | 否 |
| PATCH | /api/groups/{id} | 更新群组部分信息 | 是 | 否 |
| DELETE | /api/groups/{id} | 删除指定群组 | 是 | 否 |
| GET | /api/groups/{id}/members | 获取指定群组下的成员 | 是 | 是 |
| GET | /api/groups/{id}/members/{memberId} | 获取指定群组下的指定成员 | 是 | 是 |

幂等性：同一个RESTful接口的多次访问，得到的资源状态是相同的。

安全性：对该RESTful接口访问，不会使服务端资源的状态发生改变。
