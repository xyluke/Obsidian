# nginx的gz文件如何访问
![[../img/Pasted image 20240326104459.png]]
# 参考
[nginx的问题](https://www.cnblogs.com/wyd168/p/6636529.html)


# 示例配置
~~~c
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

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
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

     server {
        listen       8999;
        server_name  127.0.0.1;
        listen       [::]:8999;
        root         E:/nginx-1.22.1/portal-device;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            #允许跨域请求的域，* 代表所有
            add_header 'Access-Control-Allow-Origin' *;
            #允许带上cookie请求
            add_header 'Access-Control-Allow-Credentials' 'true';
            #允许请求的方法，比如 GET/POST/PUT/DELETE
            add_header 'Access-Control-Allow-Methods' *;
            #允许请求的header
            add_header 'Access-Control-Allow-Headers' *;
            root   E:/nginx-1.22.1/portal-device;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    # server {
    #     listen       8088;
    #     server_name  127.0.0.1;
    #     listen       [::]:8088;
    #     root         E:/nginx-1.22.1/oa;

    #     #charset koi8-r;

    #     #access_log  logs/host.access.log  main;

    #     location / {
    #         #允许跨域请求的域，* 代表所有
    #         add_header 'Access-Control-Allow-Origin' *;
    #         #允许带上cookie请求
    #         add_header 'Access-Control-Allow-Credentials' 'true';
    #         #允许请求的方法，比如 GET/POST/PUT/DELETE
    #         add_header 'Access-Control-Allow-Methods' *;
    #         #允许请求的header
    #         add_header 'Access-Control-Allow-Headers' *;
    #         root   E:/nginx-1.22.1/oa;
    #         index  index.html index.htm;
    #         try_files $uri $uri/ /index.html;
    #     }

    #     #error_page  404              /404.html;

    #     # redirect server error pages to the static page /50x.html
    #     #
    #     error_page   500 502 503 504  /50x.html;
    #     location = /50x.html {
    #         root   html;
    #     }
    # }

    server {
        listen       8089;
        server_name  127.0.0.1;
        listen       [::]:8089;
        root         E:/nginx-1.22.1/myVform;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            #允许跨域请求的域，* 代表所有
            add_header 'Access-Control-Allow-Origin' *;
            #允许带上cookie请求
            add_header 'Access-Control-Allow-Credentials' 'true';
            #允许请求的方法，比如 GET/POST/PUT/DELETE
            add_header 'Access-Control-Allow-Methods' *;
            #允许请求的header
            add_header 'Access-Control-Allow-Headers' *;
            root   E:/nginx-1.22.1/myVform;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
        }

        location /myVform/externalVform {
            location ~* \.js$ {
                expires off;
            }
        }

        location /externalVform/ {
            #root E:/nginx-1.22.1/myVform/externalVform/;
            alias E:/nginx-1.22.1/myVform/externalVform/;
            autoindex on;
            #允许跨域请求的域，* 代表所有
            add_header 'Access-Control-Allow-Origin' *;
            #允许带上cookie请求
            add_header 'Access-Control-Allow-Credentials' 'true';
            #允许请求的方法，比如 GET/POST/PUT/DELETE
            add_header 'Access-Control-Allow-Methods' *;
            #允许请求的header
            add_header 'Access-Control-Allow-Headers' *;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    server {
        listen       8088;
        server_name  127.0.0.1;
        listen       [::]:8088;
        root         E:/nginx-1.22.1/myVform2;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        gzip  on;

        location / {
            #允许跨域请求的域，* 代表所有
            add_header 'Access-Control-Allow-Origin' *;
            #允许带上cookie请求
            add_header 'Access-Control-Allow-Credentials' 'true';
            #允许请求的方法，比如 GET/POST/PUT/DELETE
            add_header 'Access-Control-Allow-Methods' *;
            #允许请求的header
            add_header 'Access-Control-Allow-Headers' *;
            root   E:/nginx-1.22.1/myVform2;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
        }

        location /myVform2/externalVform {
            location ~* \.js$ {
                expires off;
            }
        }

        location /externalVform/ {
            #root E:/nginx-1.22.1/myVform/externalVform/;
            gunzip on;
            gzip_static on;
            gzip_types text/plain application/xml image/x-icon;
            alias E:/nginx-1.22.1/myVform2/externalVform/;
            autoindex on;
            #允许跨域请求的域，* 代表所有
            add_header 'Access-Control-Allow-Origin' *;
            #允许带上cookie请求
            add_header 'Access-Control-Allow-Credentials' 'true';
            #允许请求的方法，比如 GET/POST/PUT/DELETE
            add_header 'Access-Control-Allow-Methods' *;
            #允许请求的header
            add_header 'Access-Control-Allow-Headers' *;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

}

~~~
