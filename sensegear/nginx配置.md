```nginx

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }

    upstream webpackDev {
        server 127.0.0.1:8100;
    }

    server {
        listen       8080;
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #access_log  "pipe:rollback logs/host.access_log interval=1d baknum=7 maxsize=2G"  main;

        rewrite '^/$' /os-console/ redirect;

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # dev proxy - grafana
		location /sg-grafana/ {
			# proxy_pass  http://monitor.phoenix.sensetime.com/sensegear/grafana/;
			# proxy_pass  https://10.121.2.218/grafana/;
            proxy_pass  https://10.198.30.205/grafana/;
		}
        # proxy to http-servers
        location /os-console/ {
            proxy_pass  http://127.0.0.1:8100/;
        }
        location /os-passport/ {
            proxy_pass  http://127.0.0.1:8102/;
        }
        location /os-docs/ {
            proxy_pass  http://127.0.0.1:8103/;
        }
        location /sensebee/ {
            proxy_pass  http://127.0.0.1:8104/;
        }
        # 新增 - 本地接口调用，解决跨域
        location /alankzh/ {
            proxy_pass  https://spe-test.com/api/;
            # proxy_pass  https://spe-dev.com/api/;
            # proxy_pass  https://spe-st.com/api/;
        }
        # location /alankzh/iam/ {
        #     proxy_pass  http://10.151.93.97:8080/iam/;
        # }
        # 新增 - ws 转发
        location /sockjs-node {
            proxy_pass http://webpackDev;
            proxy_read_timeout 300s;
            proxy_send_timeout 300s;

            proxy_set_header Host $host:$server_port;
            #升级http1.1到 websocket协议
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection  $connection_upgrade;
        }
        location /dev-server {
			proxy_pass http://127.0.0.1:8104;
			proxy_read_timeout 300s;
			proxy_send_timeout 300s;

			proxy_set_header Host $host:$server_port;
			#升级http1.1到 websocket协议
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection  $connection_upgrade;
		}


    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    include servers/*;
}

```

