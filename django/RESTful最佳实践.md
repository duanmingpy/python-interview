# RESTful最佳实践

[返回目录](https://github.com/duanmingpy/python-interview/blob/master/django/%E7%9B%AE%E5%BD%95.md)         
[返回首页](https://github.com/duanmingpy/python-interview)           
REST（Representational State Transfer），表现层状态转移。

它首次出现在2000年`Roy Fielding`的博士论文中，Roy Fielding是HTTP规范的主要编写者之一。
表现层是`资源`的表现层，对于网络中的资源就需要URI(Uniform Resource Identifier)来指向。

### 1. 协议

对外若有安全性要求，可以使用HTTPS；但是内部服务间调用可以使用HTTP或
HTTPS。

### 2. HTTP方法

| 常用方法（动词） | 说明         |
| ---------------- | ------------ |
| GET              | 获取资源     |
| POST             | 创建新的资源 |
| PUT              | 更新资源     |
| PATHCH           | 部分更新资源 |
| DELETE           | 删除资源     |

### 3. 使用名词

URL指向`资源`，在URL路径的描述中，只需要出现名词，而`不要`出现动词。动词由HTTP方法提供。
不要单复数混用，建议名词使用复数。
Restful的`核心`是`资源`，URL应该指向资源，所以应该是使用名称表达，而不是动词表达。

| 方法   | 路径      | 说明                     |
| ------ | --------- | ------------------------ |
| GET    | /posts    | 返回所有文章             |
| GET    | /posts/10 | 返回id为10的文章         |
| POST   | /posts    | 创建新的文章             |
| PUT    | /posts/10 | 更新id为10的文章         |
| DELETE | /posts/10 | 删除id为10的文章         |
| PATCH  | /posts/10 | 部分更新id为10的文章数据 |

不要出现下面的访问资源的路径：

```
/getAllPosts
/addPost
/updatePost
/delPost
```

GET方法只是获取资源，而不是改变资源状态。改变资源请使用POST、PUT、DELETE等方法。

例如，使用`GET /posts/10` 就可以获取资源了，但是却使用`GET /posts/10/del` 或`GET /posts/10?
v=del` ，本意是想删除。但这样不好，GET方法请求只为`获取`资源，不要`改变`资源状态。

子资源的访问：

| 方法 | 路径Endpoint        | 说明                                |
| ---- | ------------------- | ----------------------------------- |
| GET  | /posts/10/authors   | 返回id为10的文章的所有作者          |
| GET  | /posts/10/authors/8 | 返回id为10的文章的作者中id为4的文章 |

### 4. 集合功能

**过滤Filtering：**指定过滤条件`GET /posts?tag=python`；

**排序Sorting：**指定排序条件，有很多种设计风格，例如使用+表示asc，-表示desc；`GET /posts?sort=+title,-id`或`GET /posts?sort=title_asc,id_desc`。

**分页Pagination：**一般情况下，查询返回的记录数非常多，必须进行分页，`GET /posts?page=5&size=20`。

### 5. 状态码

使用HTTP响应的状态码表示动作的成功与否;`2XX`表示用户请求被服务器端成功的处理；`4XX`表示用户请求的错误；`5XX`表示服务器端出错了，[参考文档](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

| Status Code | 说明                  | Method           | 说明                                                         |
| ----------- | --------------------- | ---------------- | ------------------------------------------------------------ |
| 200         | OK                    | GET              | 成功获取资源                                                 |
| 201         | CREATED               | POST、PUT、PATCH | 成功创建或修改                                               |
| 204         | NO CONTENT            | DELETE           | 成功删除资源                                                 |
| 400         | Bad Request           | ALL              | 请求中有错误，例如GET时参数有问题；<br>PUT时提交的数据错误等 |
| 401         | Unauthorized          | ALL              | 权限未通过认证                                               |
| 403         | Forbidden             | ALL              | 有无权限都禁止访问该资源                                     |
| 404         | Not Found             | ALL              | 请求的资源不存在                                             |
| 500         | Internal Server Error | ALL              | 服务端错误                                                   |

### 6. 错误处理

在`Restful API`设计中，错误处理也非常重要，单单从状态码中无法详尽描述错误的信息。

1. 返回消息

```json
{
error:"User Not Found"
}
```

2. 从错误消息中了解到错误号、错误信息、错误描述等信息。甚至更详细的信息可以通过code查阅文
   档。

```json
{
"code":10056,
"message":"Invalid ID",
"description":"More details":
}
```

code 为 0 表示无错误。非0表示有错误，就要看message中的错误描述了。

### 7. 版本

强烈要求使用版本，版本号使用简单数字，例如v2；两种风格：

`http://api.aliyun.com/v1/posts/10`

`http://www.aliyun.com/api/v1/posts/10`

第一种风格会跨域，适合较大的项目。

### 8. 返回结果

返回数据一律采用JSON格式。
