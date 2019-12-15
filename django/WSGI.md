# WSGI
[返回目录](https://github.com/duanmingpy/python-interview/blob/master/django/%E7%9B%AE%E5%BD%95.md)         
[返回首页](https://github.com/duanmingpy/python-interview)           

WSGI全称是Web Server Gateway Interface，是python中通用的一个网关接口，对于动态资源需要调用处理函数，WSGI就是用来连接Web Server和应用程序之间的网关接口。

![wsgi](https://github.com/duanmingpy/python-interview/blob/master/images/02WSGI.jpg)

**WEB服务器**

本质上就是一个TCP服务器，监听在特定端口上；

支持HTTP协议，能够将HTTP请求报文进行解析，能够把响应数据进行HTTP协议的报文封装并返

回浏览器端。

实现了WSGI协议，该协议约定了和应用程序之间接口（参看PEP333,`https://www.python.org/dev/peps/pep-0333/`）

**服务器端APP应用程序**

遵从WSGI协议

本身是一个可调用对象

调用start_response，返回响应头部

返回包含正文的可迭代对象

WSGI 框架库往往可以看做增强的更加复杂的Application。

### wsgiref

wsgiref是python提供的一个WSGI参考实现库，不适合生产环境使用。wsgiref.simple_server模块实现一个简单的WSGI　HTTP服务器。

```python
# wsgiref库的simple_server模块的make_server函数的签名：
wsgiref.simple_server.make_server(host, 
                                    port, 
                                    app, 
                                    server_class=WSGIServer,
                                    handler_class=WSGIRequestHandler)

# demo_app的签名，demp_app代表所有app的类型
wsgiref.simple_server.demo_app(environ, start_response)
```

例子：

```python
from wsgiref.simple_server import make_server, demo_app

ip = '127.0.0.1'
port = 9999
server = make_server(ip, port, demo_app)
server.serve_forever()   # server.handle_request() 执行一次
```

WSGI 服务器作用：

1. 监听HTTP服务端口（TCPServer，默认端口80）；

2. 接收浏览器端的HTTP请求并解析封装成environ环境数据；

3. 负责调用应用程序，将environ数据和start_response方法两个实参传入给Application ；

4. 将应用程序返回的正文封装成HTTP响应报文返回浏览器端。

### WSGI APP应用程序

1. 这个app应用程序应该是一个可调用对象，python中应该是函数、类、实现了__call__方法的类的实例；

2. 这个可调用对象应该接收两个参数。

```python
from wsgiref.simple_server import make_server, demo_app


# 要求使用三种方式
def my_app(environ, start_response):
    start_response("200 OK", [("Content-Type", "Text/plain; charset=utf-8")])
    return [b'ascd111ddd', b'333333']


class App:
    def __init__(self, environ, start_response):
        self.environ = environ
        self.start_response = start_response

    def __iter__(self):
        self.start_response("200 OK", [("Content-Type", "Text/plain; charset=utf-8")])
        yield from iter(map(lambda x:x.encode(), ["我", "是", "谁"]))


class HerApp:
    def __call__(self, *args, **kwargs):
        environ, start_response = args
        start_response("200 OK", [("Content-Type", "Text/plain; charset=utf-8")])
        return [b"nihao"]

ip = '127.0.0.1'
port = 9999
server = make_server(ip, port, HerApp())
server.serve_forever()
```

environ和start_response这两个参数名可以是任何合法名，但是一般默认都是这2个名字；应用程序端还有些其他的规定，暂不用关心。

**environ**

environ是包含Http请求信息的dict字典对象，字典中一些重要的键值对：

| 名称                    | 含义                  |
| ----------------------- | --------------------- |
| **REQUEST_METHOD**      | 请求方法，GET、POST等 |
| **PATH_INFO**           | URL中的路径部分       |
| **QUERY_STRING**        | 查询字符串            |
| SERVER_NAME,SERVER_PORT | 服务器名、端口        |
| HTTP_HOST               | 地址和端口            |
| SERVER_PROTOCOL         | 协议                  |
| HTTP_USER_AGENT         | UserAgent信息         |

```
CONTENT_TYPE = 'text/plain'
HTTP_HOST = '127.0.0.1:9999'
HTTP_USER_AGENT = 'Mozilla/5.0 (Windows; U; Windows NT 6.1; zh-CN) AppleWebKit/537.36
(KHTML, like Gecko) Version/5.0.1 Safari/537.36'
PATH_INFO = '/'
QUERY_STRING = ''
REMOTE_ADDR = '127.0.0.1'
REMOTE_HOST = ''
REQUEST_METHOD = 'GET'
SERVER_NAME = 'DESKTOP-D34H5HF'
SERVER_PORT = '9999'
SERVER_PROTOCOL = 'HTTP/1.1'
SERVER_SOFTWARE = 'WSGIServer/0.2'
```

**start_response：**

它是一个可调用对象。有3个参数，定义如下：

`start_response(status, response_headers, exc_info=None)`

| **参数名称**     | **说明**                                                     |
| ---------------- | ------------------------------------------------------------ |
| status           | 状态码和描述状态，例如200 OK                                 |
| response_headers | 一个元素为二元组的列表，例如[('Content-Type', 'text/plain; charset=utf-8')] |
| exc_info         | 在错误处理的时候使用                                         |

 start_response应该在返回可迭代对象之前调用，因为它是Response Header。返回的可迭代对象是

Response Body。

### 整体实现

服务器程序需要调用符合上述定义的可调用对象APP，传入environ、start_response，APP处理后，返回响应头和可迭代对象的正文，由服务器封装返回浏览器端。

```python
from wsgiref.simple_server import make_server


# 返回网页的例子
def application(environ, start_response):
    status = '200 OK'
    headers = [("Content-Type", "text/html; charset=utf-8")]
    start_response(status, headers)
    # 返回可迭代对象
    html = '<h1>welcome to the 21st</h1>'.encode("utf-8")
    return [html]


ip = '127.0.0.1'
port = 9999
server = make_server(ip, port, application)
server.serve_forever()
```

测试命令：

```shell
curl -I http://192.168.116.66:9999/xxx?id=5
curl -X POST http://192.168.142.1:9999/yyy -d '{"x:2}'
```

-I 使用HEAD方法  -X 指定方法，-d传输数据，到这里就完成了一个简单的WEB 程序开发。

