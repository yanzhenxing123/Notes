## 反向代理

**正向代理**：在客户端配置代理服务器

**反向代理**：所谓反向代理正好与正向代理相反，代理服务器是为目标服务器服务的，虽然整体的请求返回路线都是一样的都是Client到Proxy到Server。

只需要将请求发送给反向代理服务器就可以了， 对外暴露的就是代理服务器，真的服务器被隐藏了。



## 负载均衡

增加服务器的数量，将请求平均的分配到服务器上。

![image-20200704175209079](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/111603-564318.png)



## 动静分离

将动态资源和静态资源分开进行设置，可以将动态页面和静态页面由不同的服务器进行解析，加快解析速度。降低原来单个服务器的压力 。

![image-20200704175708963](https://raw.githubusercontent.com/yanzhenxing123/blogImg/master/typora202008/22/111610-344614.png)





*其他*：linux中/是root目录，~则是root目录，包含于根目录



## 我的现在配置

```shell
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
	worker_connections 768;
	# multi_accept on;
}

http {

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;

	# server_tokens off;

	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;

	# gzip_vary on;
	# gzip_proxied any;
	# gzip_comp_level 6;
	# gzip_buffers 16 8k;
	# gzip_http_version 1.1;
	# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

	##
	# Virtual Host Configs
	##

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
	server {
         	listen     80;
		server_name www.yanzx.top;

       	 	#root        /usr/share/nginx/html;
		#location / {
                #        proxy_pass      http://127.0.0.1:8001;
     		#}
		
        	 # 用wsgi部署的python Django blog项目（主项目）
  		location / {
  			proxy_set_header Host $host;
    			proxy_pass 
			http://unix:/tmp/39.98.126.80.socket;  # 改成你的 IP
  		}

		 location /static {
    			alias /home/sites/yanzxphoto.com/DjangoBlog/collected_static;
 		 }

  		location /media {
    			alias /home/sites/yanzxphoto.com/DjangoBlog/media;
 		}
		

		location ^~ /index {
			proxy_pass http://127.0.0.1:8080;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		}

		location ^~ /index/static {
			alias /home/gzs/static;
                }
		location ^~ /commit {
			proxy_pass http://127.0.0.1:8080;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		}
		# 请求的url为39.98.126.80/bank
		 #location ^~ /bank {
		#	proxy_pass http://127.0.0.1:8000;
		#	proxy_set_header Host $host;
		#	proxy_set_header X-Real-IP $remote_addr;
		#	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		#}
       }

	server{
        	listen 80;
       		server_name abc.yanzx.top;
        
        	location ^~{
        	proxy_pass http://127.0.0.1:8000;
        	proxy_set_header Host $host;
        	proxy_set_header X-Real-IP $remote_addr;
        	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

		 location /static {
    			alias /home/sites/wb/static;
 		 }

  		location /media {
    			alias /home/sites/wb/media;
 		}


	}


}
	

}




#mail {
#	# See sample authentication script at:
#	# http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
#	# auth_http localhost/auth.php;
#	# pop3_capabilities "TOP" "USER";
#	# imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
#	server {
#		listen     localhost:110;
#		protocol   pop3;
#		proxy      on;
#	}
# 
#	server {
#		listen     localhost:143;
#		protocol   imap;
#		proxy      on;
#	}
#}

```

**默认配置**



```bash

#定义Nginx运行的用户和用户组
#user  nobody; 

#nginx进程数，建议设置为等于CPU总核心数。
worker_processes  1; 

#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#进程文件
#pid        logs/nginx.pid;

#工作模式与连接数上限
events {
    #单个进程最大连接数（最大连接数=连接数*进程数）
    worker_connections  1024;
}

#设定http服务器
http {
    #文件扩展名与文件类型映射表
    include       mime.types;
    #默认文件类型
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改 成off。
    sendfile        on;

    #防止网络阻塞
    #tcp_nopush     on;


    #长连接超时时间，单位是秒
    #keepalive_timeout  0;
    keepalive_timeout  65;

    #开启gzip压缩输出
    #gzip  on;

    #虚拟主机的配置
    server {
        #监听端口
        listen       80;

        #域名可以有多个，用空格隔开
        server_name  localhost;

        #默认编码
        #charset utf-8;

        #定义本虚拟主机的访问日志
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

}

```

