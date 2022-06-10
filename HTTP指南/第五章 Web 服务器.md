http权威指南 第五章

# Web 服务器

web 服务器的任务

- 建立连接
- 接收请求
- 处理请求
- 访问资源
- 构建响应
- 发送响应
- 记录事务的处理过程——日志

## 5.4 建立连接

如果客户端已经打开了一条到服务器的持久连接，可以使用那条连接发送请求。否则，客户端需要打开一条新的连接

### 5.4.1 处理新连接

客户端请求到服务器的TCP连接时，web服务器会建立连接，从TCP中解析IP地址，判断另一端是哪一个客户端。

一旦一条新连接建立起来并接受，服务器就会将新连接添加到其现存的到连接列表中，准备监视它上面的数据

web服务器可以随意拒绝或立即关闭任意一条连接



### 5.4.2 客户端主机识别

可以利用反向DNS将客户端的IP地址转换成客户端主机名。

web服务器将客户端主机名用于日志记录



### 5.4.3 通过ident 确定客户端用户

web服务器可以通过ident协议找到发起http连接的用户名



## 5.5 接收请求

- 解析请求行 
- 读取以CRLF结尾的header
- 检测以CRLF结尾的、标识首部结束的空行
- 如果有读取请求主体，其长度由content-length 决定

```
GET /static/background.png HTTP/1.1
Host: localhost:3000
Connection: keep-alive
Cache-Control: max-age=0
```



## 5.6 处理请求

略



##  5.7 对资源的映射及访问

web服务器负责发送预先创建好的内容，或者运行在服务器的资源生成程序所产生的动态内容



### 5.7.1 docroot

web服务器的文件系统中会有一个特殊的文件夹存放web内容。这个文件被称为文档的根目录 docement root. web服务器从请求报文中获取URI，并将其附加在文档根目录的后面。

**虚拟托管的docroot**

这类web服务器会在同一台web服务器上提供多个web站点，每个站点在服务器上都有自己独有的文档根目录

```
GET /index.html HTTP/1.0
Host: www.joes-hardware.com
----> /docs/joes

GET /index.html HTTP/1.0
Host: www.joes-hardware.com
-----> /docs/mary
```



### 5.7.2 目录列表

web服务器接收对目录的URL请求 其路径解析为一个目录，而不是文件



### 5.7.3 动态资源的映射

web服务器还可以将URI映射为动态资源，也就是说映射到按需动态生成内容的程序上去

在Apache中允许用户将URI路径名组件映射为可执行文件目录

```
Script Alias /cgi-bin/ /usr/local/etc/httpd/cgi-programs/
```

或者以特殊的文件后缀名来标识可执行文件

```
AddHandler cgi-script.cgi
```



## 5.8 构建响应

响应报文中包含响应状态码、响应首部和响应主体（如果有的话）



### 5.8.2 响应主体

响应主体

如果产生响应主体，响应报文的响应头中应该包含

- 描述响应主体MIMIE 类型的 Content-Type 
- 描述响应主体长度的 Content-Length 首部
- 实际报文的主体内容



**MIME 类型**

- mime.type 

web服务器可以用文件的扩展名来说明mime类型

```
application/msdoc doc
image/gif 				gif
text/html 				html htm
text/plain				txt
```

- magic type
- explicit typing
- 类型协商



### 5.8.3 重定向

- 永久搬离的资源
- 临时搬离的资源
- URL增强
- 负载均衡
- 服务器关联
- 规范目录名称

