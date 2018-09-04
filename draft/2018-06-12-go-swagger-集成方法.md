---
title: "Go Swagger 集成方法"
date: 2018-06-12T15:11:07+08:00
tags: []
categories: []
---

## 目录
>  
1、   
2、xx


### 1、Swagger:meta 描述项目信息

### 2、Swagger:route 描述简单路由
有两种注释路由的方法，使用 `swagger:operation` 和 `swagger:route`。`swagger:route`适用于简单的 api，`swagger:operation`适用于有查询参数的 api，比如 `/repos/{owner}`。



### 3、Swagger:operation 描述复杂路由
/user/{id} or /users/search?name=ribice

in:path
in:query


`After defining your routes, you need to define your requests and responses.`  
`定义好了路由，接下来我们定义请求(requests)和响应(responses)`   



### 3、Swagger:
3种

```
// 添加 link 的请求参数定义 写点什么注释好呢
// swagger:parameters AddLinkRequest
type swaggerAddLinkRequest struct {
	// 网页链接
	//
	// in:query
	Url string `json:"url"`
	// 类别 ID
	//
	// in:body
	CategoryId string `json:"categoryId"`
	// 标签
	//
	// in:query
	Tag string `json:"tag"`
}
```


```
1 // Request containing string
2 // swagger:parameters createRepoReq
3 type swaggerCreateRepoReq struct {
4 // in:body
5 api.CreateRepoReq
6 }
```

```
 // Request containing string
2 // swagger:parameters createRepoReq
3 type swaggerCreateRepoReq struct {
4 // in:body
5 Body api.CreateRepoReq
6 }
```





// swagger:operation GET /api/v1/getLinksByCategoryId link NoParamsRequest
// ---
// summary: 根据 categoryId 获取 link.
// description: 该填点什么描述好呢.
// parameters:
// - name: categoryId
//  in: query
//  description: 类别 ID
//  type: string
//  required: true
// responses:
//  "0":
//     description: xxx
//     schema:
//       items:
//         "$ref": "#/responses/BasicFailedResponse"
// responses:
//  "1":
//     description: xxx
//     schema:
//       items:
//         "$ref": "#/responses/GetLinksResponse"


// 根据 categoryId 获取 link
//
// swagger:route GET /api/v1/getLinksByCategoryId link GetLinksByCategoryIdRequest
// 该接口实现了根据 categoryId 获取 link.
// 该填点什么描述好呢.
// responses:
//   0: BasicFailedResponse
//   1: GetLinksResponse
