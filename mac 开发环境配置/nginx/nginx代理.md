在node中运行以下代码

```js
var http = require('http');
var url = require('url');
var util = require('util');

http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end(util.inspect(url.parse(req.url, true)));
}).listen(3000);
```

nginxconf

```nginx
events {

}
http {

  # proxy_server
  server {
    listen 8080;
    location / {
      proxy_pass http://localhost:3000/;
    }

    location ~ \.(gif|jpg|png)$ {
      root /Users/zhuyifei/Documents/learn-nginx/images;
    }
  }
}
```

可以通过代理访问到 nodejs 上的服务器