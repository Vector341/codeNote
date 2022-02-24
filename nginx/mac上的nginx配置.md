# Mac 上的 nginx 配置

官方教程 [Beginner’s Guide (nginx.org)](http://nginx.org/en/docs/beginners_guide.html)

针对 在 Mac 上使用 homebrew 安装的 nginx 

系统信息 macOS Monterey 

MacBook Air M1, 2020

**在 mac 中 nginx 相关文件的位置与 linux 不同**

/opt/homebrew/etc/nginx/nginx.conf —— 配置文件

/opt/homebrew/Cellar/nginx/1.21.6/ ——主目录

/opt/homebrew/var/log/nginx/error.log —— 日志

/opt/homebrew/var/log/nginx/access.log



## 启动、停止与重启命令

启动需要借助brew

```
brew services restart nginx
```

启动后的其他操作如下

> ```
> nginx -s signal
> ```

Where *signal* may be one of the following:

- `stop` — fast shutdown
- `quit` — graceful shutdown
- `reload` — reloading the configuration file
- `reopen` — reopening the log files

常用命令 To reload configuration, execute:

> ```
> nginx -s reload
> ```



## 配置文件结构 nginx.conf

Directives are divided into simple directives and block directives. A simple directive consists of the name and parameters separated by spaces and ends with a semicolon (`;`). A block directive has the same structure as a simple directive, but instead of the semicolon it ends with a set of additional instructions surrounded by braces (`{` and `}`). If a block directive can have other directives inside braces, it is called a context (examples: [events](http://nginx.org/en/docs/ngx_core_module.html#events), [http](http://nginx.org/en/docs/http/ngx_http_core_module.html#http), [server](http://nginx.org/en/docs/http/ngx_http_core_module.html#server), and [location](http://nginx.org/en/docs/http/ngx_http_core_module.html#location)).



## 静态内容服务

```nginx
events {
  
}
http {
  server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
	}
}

```

Index.html 文件应位于 /data/www 内

example.png 图片应位于 /data/images/

For example, in response to the `http://localhost/images/example.png` request nginx will send the `/data/images/example.png` file. 

Tip:

在location 中设置 root 指向需要的文件时，不要使用 ～ 代表 home 路径



## 代理服务器

```nginx
events {

}
http {
  # normal server 也可以是其他服务（如nodejs等）启动的服务器
  server {
    listen 8080;
    root /Users/zhuyifei/Documents/learn-nginx;
    location / {
    }
  }

  # proxy_server
  server {
    listen 3000;
    location / {
      proxy_pass http://localhost:8080/;
    }

    location ~ \.(gif|jpg|png)$ {
      root /Users/zhuyifei/Documents/learn-nginx/images;
    }
  }
}
```

对应目录结构为

```
learn-nginx
├── images

│  └── screenshot.png

├── index.html

└── subDir

  └── sub.html
```

访问 `localhos:3000` 则代理服务器会代理请求至普通服务器 `localhost:8080`

这里使用 nginx 启动普通服务器，可以用 nodejs 等其他工具启动这项服务



`localhost:3000/screenshot.png` 直接访问本地图片 screenshot



## nginx 调试相关

Ctrl + shift + R 忽略缓存 强制刷新 hard refresh

Press Cmd+Shift+R to hard refresh your browser in case you are seeing some cached content.