# Nginx config.xml 의 위치
- etc/nginx


```
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;
	
    upstream member {
		server 172.17.0.1:8001;

    }

	upstream trainer {
		server 172.17.0.1:8002;
	}
	

    server {
		listen 80;
		server_name localhost;
        server_tokens off;

		location = /trainer/ {
			proxy_pass	http://trainer;
			proxy_redirect	off;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header X-Forwarded-Host $server_name;
		}

        location = /member/ {
			proxy_pass	http://member;
			proxy_redirect	off;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header X-Forwarded-Host $server_name;
		}
		
	}

	

    include /etc/nginx/conf.d/*.conf;
}
```