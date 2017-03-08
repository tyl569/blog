---
title: Nginx根据端口配置虚拟主机
date: 2016-07-31 15:01:48
tags: Nginx 
categories: Linux
---

#### 1)  打开nginx的配置文件，为了方便日后对虚拟主机进行管理，我们可以创建一个文件夹 vhost，专门存放虚拟主机的配置文件 

```nginx
...
include vhost/*.conf;
}
```

#### 2) 在vhost文件夹，创建blog.conf

> 我们根据端口进行配置
> 配置8080端口，指向 blog文件夹

```nginx 
server {
        listen       8080;
        server_name  localhost:8080 ;
        root   "D:/blog";
        location / {
            index  index.html index.htm index.php;
        }
        location ~ \.php(.*)$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_split_path_info  ^((?U).+\.php)(/?.+)$;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            fastcgi_param  PATH_INFO  $fastcgi_path_info;
            fastcgi_param  PATH_TRANSLATED  $document_root$fastcgi_path_info;
            include        fastcgi_params;
        }
}
```

#### 3) 重启nginx服务，浏览器访问 http://localhost:8080