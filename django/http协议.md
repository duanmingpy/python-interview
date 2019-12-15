# http协议

### web开发

**CS:** CS即客户端、服务器编程。客户端、服务端之间需要使用`Socket`，约定协议、版本（往往使用的协议是`TCP或者UDP`），指定地址和端口，就可以通信了。客户端、服务端传输数据，数据可以有一定的格式，双方必须先约定好。

**BS:** BS编程，即Browser、Server开发。Browser浏览器，一种特殊的客户端，支持`HTTP(s)`等协议，能够通过URL向服务端发起请求，等待服务端返回HTML等数据，并在浏览器内可视化展示的程序。Server，支持HTTP(s)协议，能够接受众多客户端发起的HTTP协议请求，经过处理，将HTML等数据返回给浏览器。    

本质上来说，BS是一种特殊的CS，即`客户端`必须是一种`支持HTTP协议`且能`解析并渲染HTML`的软件，服务端必须是能够接收`多客户端HTTP访问`的服务器软件。HTTP协议底层基于TCP协议实现。 

### http协议

可以安装httpd或nginx等服务端服务程序，通过浏览器访问，观察http协议。

**http的三个特点：**

**无状态：**指的是服务器无法知道2次请求之间的联系，即使是前后2次同一个浏览器也没有任何数据能够判断出是同一个浏览器的请求。后来可以通过`cookie`、`session`来判断。      

**有连接：** 是因为它基于`TCP`协议，是面向连接的，需要3次握手、4次断开。        

**短连接：**Http1.1之前，都是一个请求一个连接，而`Tcp的连接创建销毁成本高`，对服务器有很大的影响。所以，自Http1.1开始，支持`keep-alive`，默认也开启，一个连接打开后，会保持一段时间（可设置），浏览器再访问该服务器就使用这个Tcp连接，减轻了服务器压力，提高了效率。    

 Http协议是无状态协议。同一个客户端的两次请求之间没有任何关系，从服务器端角度来说，它不知道这两个请求来自同一个客户端。

### URL组成

URL可以说就是`地址`，`uniform resource locator`统一资源定位符，每一个链接指向一个`资源`供客户端访问。     

**格式：**`scheme://host[:port#]/path/...[?query-string][#anchor]`

**scheme 模式、协议:** `http`、`ftp`、`https`、`file`、`mailto`等等。<br>**host:port:**`www.baidu.com:80`，80端口是默认端口可以不写。域名会使用DNS解析，域名会解析成IP才能使用。实际上会对解析后返回的IP的TCP的80端口发起访问。 <br>**/path/to/resource:** `path`，指向资源的路径。<br>**?key1=value1&key2=value2:** `query string`，查询字符串，问号用来和路径分开，后面是`key=value`形式，且使用&符号分割。     

例如，通过URL访问网页: `https://visualgo.net/en/list?slide=1` <br>访问静态资源时，通过上面这个URL访问的是网站的某路径下的index.html文件，而这个文件对应磁盘上的真实的文件。就会从磁盘上读取这个文件，并把文件的内容发回浏览器端。

### http消息组成

消息分为`Request`、`Response`。`Request`是浏览器向服务器发起的请求；`Response`是服务器对客户端请求的响应。      

请求报文由`Header`消息报头、`Body`消息正文组成（可选），请求报文第一行称为请求行。<br>响应报文由`Header`消息报头、`Body`消息正文组成（可选），响应报头第一行称为状态行。<br>每一行使用`回车和换行符`作为结尾，如果有Body部分，`Header`、`Body`之间`留一行`空行。

`GET`请求报文格式：

```
GET /users/ HTTP/1.1
Host:www.baidu.com
User-Agent:Mozilla/5.0
Cookie:....
```

`POST`请求报文格式：

```
POST /xxx/yyy?id=5&name=magedu HTTP/1.1
HOST: 127.0.0.1:9999
content-length: 26
content-type: application/x-www-form-urlencoded

age=5&weight=80&height=170
```

响应报文格式：

```
HTTP/1.1 200 OK
Date:...
Content-Type:....
```

**status code状态码：**

```python
状态码在响应头第一行
1xx 提示信息，表示请求已被成功接收，继续处理
2xx 表示正常响应
    200 OK 正常返回了网页内容
    201 Created 请求已经被实现，依据请求要求，已经创建了新的资源
    204 No Content 服务器端成功处理了，但没什么内容返回

3xx 重定向
    301 Moved Permanently 页面永久性移走，永久重定向。返回新的URL，
    浏览器会根据返回的url发起新的request请求
    302 Temporary Redirect 临时重定向
    304 Not Modified 资源未修改，浏览器使用本地缓存
    307 Temporary Redirect 与302很相似，只是客户端保持当前请求方法不变重定向

4xx 客户端请求错误
    404 Not Found，网页找不到，客户端请求的资源有错
    400 Bad Request 请求语法错误
    401 Unauthorized 请求要求身份验证
    403 Forbidden 服务器拒绝请求

5xx 服务器端错误
    500 Internal Server Error 服务器内部错误
    502 Bad Gateway 上游服务器错误，例如nginx反向代理的时候
```

这一部分更详细内容参考GitHub仓库[链接地址](https://github.com/duanmingpy/python-interview/blob/master/markdowns/3-网络相关要点.md#5)

### cookie

cookie是一种嵌套的键值对信息，是一种客户端、服务端传递数据的技术，cookie曾经是唯一在浏览器端存储数据的手段，目前浏览器端存储数据的方案很多，cookie正在被淘汰，当服务器收到http请求时，服务器可以在`响应头`里添加一个`set-cookie`键值对，浏览器收到响应后通常会保存这些cookie，之后对该服务器每一次请求中都通过cookie请求头部将cookie信息发给服务器，一般来说cookie信息是在服务端生成返回给客户端的，但是客户端也可以伪造，浏览器端可以保持这些值（可长久保存，可过期保存），浏览器对`同一域`（不同的域带上不同的cookie，这都是浏览器做的，所以要用正版）每一次发起请求时都会把`cookie`信息发送给服务端，服务端收到浏览器发来的cookie，处理这些信息就可以判断这次请求是否和之前的请求有关联。另外，cookie的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

例如：

```python
Set-Cookie:aliyungf_tc=AQAAAJDwJ3Bu8gkAHbrHb4zlNZGw4Y; Path=/; HttpOnly
set-cookie:test_cookie=CheckForPermission; expires=Tue, 19-Mar-2018 15:53:02 GMT;
path=/; domain=.doubleclick.net
Set-Cookie: BD_HOME=1; path=/
```

这里面的HttpOnly表示只能够用http使用，不能用js访问，这些机制全凭客户端实现。

**cookie过期：**cookie可以设定过期时间，过期后将被浏览器清除，如果缺省的话cookie不会持久化，会话级cookie，在浏览器关闭后cookie消失。<br>**cookie域：**域确定有哪些域可以`存取`这个cookie,缺省设置属性值为当前主机，例如`www.baidu.com`,如果设置为`baidu.com`则还包含子域。<br>**Path:**确定哪些目录及子目录访问可以使用该cookie。<br>**Secure:**表示cookie随着https加密过的请求发送给服务端。<br>**HttpOnly：**将cookie设置为此标记之后将不能被JavaScript访问了，只能发给服务端。

**使用Domain和Path定义cookie的作用域：**

```python
Cookie的作用域：Domain和Path定义Cookie的作用域

Domain
domain=www.csdn.com 表示只有该域的URL才能使用
domain=csdn.com 表示可以包含子域，例如www.csdn.com、python.csdn.com等

Path
path=/ 所有/的子路径可以使用
domain=www.csdn.com; path=/webapp 表示只有www.csdn.com/webapp下的URL匹配

例如
http://www.csdn.com/webapp/a.html就可以
```

Cookie一般`明文传输`（Secure是加密传输），安全性极差，不要传输敏感数据，有`4kB大小`限制，每次请求中都会发送Cookie，增加了流量。



---

### 补充总结：

加密分为对称加密、不对称加密；

对称加密：

​	单向加密，如`md5`、`sha`把明文转换成密文，不可解密不可逆，不可还原，具有幂等性，原文一样，加密后密文一样，是hash值，一串长长的密文，是一个大整数，这个整数会被转成16进制；其中md5已经没有安全性可言了，因为计算速度太快，可以穷举所有可能性，建立一张所有可能的字符串称为彩虹表。

非对称加密：

​	https 其中s可以认为是ssl，tls是ssl的升级版，都是用openssl因为开源，有证书、公钥、私钥，公钥加密，私钥解密；私钥加密，公钥解密。

之前有一个`滴血漏洞`，影响了整个互联网，因为使用了一个C的copy函数，这个函数默认情况下默认不检查边界，Java和python是进行边界检查的，C和C++是靠指针的，当时罗永浩第一场发布会的100w人民币都捐给了openssl这个组织。



sessionid的目的：

​		`sessionid`的意思是会话id，为了防止浏览器冒充，有了sessionid的cookie是`会话级cookie`，sessionid由服务器端发出并记录，浏览器端返回的cookie当中如果带了sessionid，服务端就使用类似于字典的数据结构进行验证；这些数据都在服务器端的内存中，如果找不到对应的，服务端会重新通过set-cookie派发一个sid并记录下来，浏览器收到后会覆盖原来的sid，浏览器关闭之后，sid会消失，默认服务器端对session管理都有过期，一般15分钟或者30分钟。

​		我们在服务端写代码的时候会把很多对应数据放在与当前sid相关的session上，里面保存的是kv对，这将进一步增大对服务器内存的使用，如果访问过载，内存使用可能耗尽，可以使用代理比如`nginx`进行`load-balance负载均衡`调控。

​		因为sessionid是放在内存中，所以更严重的问题是如果服务器进程崩了则这些数据都没有了，可以使用`高可用`进行保证，因为session是`kv对`，所以可以使用`redis`。

​		当然如果在高并发的场景下可以考虑无sessionid的方案。