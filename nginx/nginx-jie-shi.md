# nginx解释

{% embed url="https://blog.csdn.net/crazw/article/details/9900669?utm\_medium=distribute.pc\_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&depth\_1-utm\_source=distribute.pc\_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control" %}

## brew安装后，配置文件位置/usr/local/etc/nginx/nginx.conf ，macOS，先研究下server的部分

```text
server {
        listen       8081;
        server_name  nginxtest;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location /code {
                proxy_set_header Host $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://13.210.109.227:8080/demo/verification/haoYangMao;
           # root   html;
           # index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```

listen这里就是指nginx的监听端口

server\_name，这里做测试的话需要把地址暂时解析到本机，这个可以在 /usr/local/etc/nginx/nginx.conf 这里配置，比如把地址www.nginx.com解析到127.0.0.1，因为本机地址现在就是nginx的服务器地址

location后面加的/code就是请求中后面加/code会从这个地方处理，比如www.nginx.com:8081/code ，有点像spring那个request mapping。

X-forward-for 是在多个代理情况下转发的地址，形式为IP0，IP1，IP2，这里的IP0就是源请求来自的地址，中间是转发的服务器的地址

proxypass指明了转发的地址，如果多个服务器同样服务的话可以放在upstream里面然后配置相关的转发策略，轮训，权重，hash进行转发



