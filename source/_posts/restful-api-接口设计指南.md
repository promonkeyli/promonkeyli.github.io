---
title: restful api 接口设计指南
tags:
  - restful api
categories:
  - restful api
date: 2024-08-30 08:54:11
---
#### 概述

1. 前后端通信，可采用restful风格的api，除了restful api还有另一种风格的api [Graphql](https://graphql.org)
2. 本质上其实就是编写http请求的一种规范

#### 常见的http方法

* GET：获取资源（请求参数一般附加在url后面）
* POST：创建新资源（请求参数一般放在请求体body中）
* PUT：更新现有资源（请求参数一般也是放在body中）
* DELETE：删除资源（删除一般是删除指定id，id放在url后面）

#### URL设计

1. url应该表示资源，而不是动作，应该使用http方法表示动作

   例子：

   * GET /users （正确）GET /getUsers（不推荐）

   * POST /users （正确）POST /createUser（不推荐）

2. 使用复数形式表示资源集合

   例子：

   * GET /users （正确）GET /user（不推荐）

#### 请求和响应设计

1. http状态码

| 分类 |                 描述                 |
| :--: | :----------------------------------: |
| 1**  |       信息性状态码，一般不常用       |
| 2**  |              成功状态码              |
| 3**  |       重定向状态码，一般不常用       |
| 4**  | 客户端错误状态码，多半是前端参数错误 |
| 5**  | 服务端错误状态码，多半是后端接口错误 |

2. 常用的状态码（**避免滥用200状态码来表示所有情况**）

|            状态码            |                      描述                      |
| :--------------------------: | :--------------------------------------------: |
|          200（OK）           |                 请求成功时使用                 |
|      400（Bad Request）      | 客户度（一般是前端）语法错误，后端服务不能理解 |
|     401（Unauthorized）      |           没有授权，需要进行身份认证           |
|       403（Forbidden）       |              访问被禁止，原因很多              |
|       404（Not Found）       |                 请求资源找不到                 |
| 500（Internal Server Error） |                 服务端内部错误                 |
|      502（Bad Gateway）      |    网关错误或者代理服务器错误，一般是挂掉了    |
|   504（Gateway Time-out）    |             网关超时，没有及时响应             |

3. 请求响应传输格式：使用JSON
4. 响应实体设计（golang为例）

```go
// Response 常规响应实体封装
type Response struct {
	/* 响应码 */
	Code StatusCode `json:"code"`
	/* 响应描述 */
	Message string `json:"message"`
	/* 响应数据(可以为空) */
	Data interface{} `json:"data,omitempty"`
	/* 响应时间戳 */
	Time int64 `json:"time"`
}

// PageResponse 列表响应实体封装
type PageResponse struct {
	/* 响应码 */
	Code StatusCode `json:"code"`
	/* 响应描述 */
	Message string `json:"message"`
	/* 响应数据(可以为空) */
	Data interface{} `json:"data,omitempty"`
	/* 响应时间戳 */
	Time int64 `json:"time"`
	/* 响应分页页码 */
	Page int `json:"page"`
	/* 响应分页大小 */
	Size int `json:"size"`
	/* 响应分页总条数 */
	Total int `json:"total"`
}

// 自定也业务状态码

type StatusCode int

const (
	// Successful responses
	StatusOK StatusCode = 200

	// Client error responses
	StatusBadRequest   StatusCode = 400
	StatusUnauthorized StatusCode = 401
	StatusForbidden    StatusCode = 403
	StatusNotFound     StatusCode = 404

	// Server error responses
	StatusInternalServerError StatusCode = 500
	StatusNotImplemented      StatusCode = 501
	StatusBadGateway          StatusCode = 502
	StatusServiceUnavailable  StatusCode = 503
)

// 实现 StatusCode 类型的string方法，会返回状态码对应的描述
func (s StatusCode) String() string {
	switch s {
	case StatusOK:
		return "OK"
	case StatusBadRequest:
		return "Bad Request"
	case StatusUnauthorized:
		return "Unauthorized"
	case StatusForbidden:
		return "Forbidden"
	case StatusNotFound:
		return "Not Found"
	case StatusInternalServerError:
		return "Internal Server Error"
	case StatusNotImplemented:
		return "Not Implemented"
	case StatusBadGateway:
		return "Bad Gateway"
	case StatusServiceUnavailable:
		return "Service Unavailable"
	default:
		return "Unknown Status"
	}
}


// 通用方法调用
// HandleOk 成功时调用
func HandleOk(c *gin.Context, code utils.StatusCode, message string, data interface{}) {
	json := &utils.Response{Code: code, Message: message, Data: data, Time: time.Now().UnixMilli()}
	c.JSON(int(code), json)
}

// HandleError 失败时调用，失败时Data为 nil
func HandleError(c *gin.Context, code utils.StatusCode, message string) {
	json := &utils.Response{Code: code, Message: message, Data: nil, Time: time.Now().UnixMilli()}
	c.JSON(int(code), json)
}
```

