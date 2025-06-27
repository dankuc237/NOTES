https://www.digitalocean.com/community/tutorials/how-to-create-temporary-and-permanent-redirects-with-apache-and-nginx
https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/?utm_medium=OSocial&utm_source=Youtube&utm_content=SOFWW&utm_id=YT-101-Apache-vs-NGINX


# NGINX
https://www.nginx.com/resources/wiki/start/

## Configuration structure
```bash
# main configuration file
/etc/nginx/nginx.conf
|__error_log /var/log/nginx/error.log; # Error log for whole NGINX
|__http { # context inside main configuration file (nginx.conf)
	|__access_log /var/log/nginx/access.log; # configured Access log for all virtual servers in HTTP server context
	|__include /etc/nginx/conf.d/*.conf; # additional configurations in HTTP context
	|__include /etc/nginx/sites-enabled/*; # all virtual servers configurations in HTTP context
	}
	
/etc/nginx/sites-available # contains all virtual server definition files 
# to link server configuration file from sites-available to sites-enabled
$ ln -s /etc/nginx/sites-available/site.example /etc/nginx/sites-enabled

# reload server command
$ sudo nginx -s reload
```
## WEB server
**`alias` vs `root`**
`alias` The entire part of the path from the request that matches the specified `location` is replaced by the value set in the `alias` directive.
`root` - When Nginx receives a request for a specific file, it appends the path from the request to the value set in the `root` directive.

### one IP:PORT - subdirs - one server
[serving-static-content](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/)
```nginx
server {
    listen 80; # listen 127.0.0.1:80; # bind to specific addres
    server_name servername.example; # not required
    root /var/www/html;
    autoindex on; # alllow directory listing, not allowed by default
    access_log /var/log/nginx/access.log; # configure additional access log, not required 
		
    location / {
        index index.html; 
    }



    location /X.txt { # get content of this one file from folder
        alias /var/www/html;
    }
    
    location /serve_file  { # serve file
        alias /var/www/html/X.txt;
    }

    location /serve_folder { # serve folder 
        autoindex on; # alllow directory listing, not allowed by default
	    alias /tmp/www/;
	    }
}
```

### one IP:PORT - subdirs - multiple servers
[reverse-proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/#introduction)
```nginx
server {
    listen 80;
    server_name myapp.example.com;
    root /var/www/html;
    location /app1/ {
        proxy_pass http://localhost:8081/;
    }
    location /app2/ {
        proxy_pass http://localhost:8082/;
    }
}
```

### one IP:PORT - subdomains - multiple servers, virtual servers
```nginx
$ cat include /etc/nginx/sites-enabled/proxy_to_google.localhost

server {
	listen 80;
	server_name proxy_to_google.localhost;
	location / {
		proxy_pass https://google.com/;
		}
	}
```
---
```nginx
$ cat include /etc/nginx/sites-enabled/proxy_to_bing.localhost

server {
        listen 80;
        server_name proxy_to_bing.localhost;
        location / {
            proxy_pass https://bing.com/;
        }
    }
```

### Load balancing
[http-load-balancer](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)


```nginx
# nginx.conf
http {
  upstream backend {
    server 127.0.0.1:8002;
    server http://app.com;
  }

  server {
    listen 80;
    # server_name www.domain.com;
    location / {
      proxy_pass http://backend;
    }
  }
}
```

## TCP/UDP streams (port forwarding)
https://nginx.org/en/docs/stream/ngx_stream_core_module.html#listen
[tcp-udp-load-balancer/](https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/)
http://nginx.org/en/docs/http/load_balancing.html
You need to install additional lib `libnginx-mod-stream`
```nginx
stream {
    server {
        listen 127.0.0.1:1234;
        proxy_pass 127.0.0.1:6363;
    }
}
```
### SSL
```nginx
stream {
    server {
        listen 12345 ssl;
        ssl_certificate /path/to/your_certificate.crt;
        ssl_certificate_key /path/to/your_private_key.key;

        proxy_pass 127.0.0.1:12346;
    }
}
```
```nginx
stream {
    server {
        listen 12345 ssl;  # Listening on port 12345 for SSL connections
        ssl_certificate /path/to/your_domain.crt;
        ssl_certificate_key /path/to/your_domain.key;

        # ssl_protocols TLSv1.2 TLSv1.3;
        # ssl_prefer_server_ciphers on;
        # ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';

        # ssl_session_cache shared:SSL:10m;
        # ssl_session_timeout 10m;

        # Other stream configuration options...
    }
}
```


## SSL/TLS terminating
[set-up-nginx-load-balancing-with-ssl-termination](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-load-balancing-with-ssl-termination)
[terminating-ssl-tcp](https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-tcp/)

1. The first block listens for HTTP requests on port 80 and redirects them to HTTPS using a 301 permanent redirect.
2. The second block listens for HTTPS requests on port 443, serves content from `/var/www/html`, and specifies SSL/TLS configuration including the certificate and key files for the domain `www.srv.world`.
```nginx
http {
    server {
        listen 80 default_server;
        listen [::]:80 default_server;
        return 301 https://$host$request_uri; # Redirects port 80/HTTP traffic to 443/HTTPS;
    }
    
    server {
       listen       443 ssl http2 default_server;
       listen       [::]:443 ssl http2 default_server;
       server_name  www.srv.world;
       root         /var/www/html;
       index index.html index.htm index.nginx-debian.html;
	   server_tokens off; # Hide Nginx version  

	   ssl on;
       ssl_certificate "/tmp/certs/fart.com.crt";
       ssl_certificate_key "/tmp/certs/fart.com.key";
       
       ssl_session_cache shared:SSL:1m;
       ssl_session_timeout  10m;
       ssl_prefer_server_ciphers       on;
	   ssl_protocols                   TLSv1 TLSv1.1 TLSv1.2;
	   ssl_ciphers                     ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS;
	   add_header Strict-Transport-Security "max-age=31536000";
	   
       location / {
              try_files $uri $uri/ =404;
       }
    }
}
```
To generate self signed cert
```bash
$ openssl genrsa -out fart.com.key 2048 # generate a private key that will be used to sign the certificate
$ openssl req -new -key fart.com.key  -out fart.com.csr # generate a Generate a Certificate Signing Request (CSR)  using the private key
$ openssl x509 -req -days 365 -in fart.com.csr  -signkey fart.com.key  -out fart.com.crt # generate a self-signed certificate using the private key and CSR
```
## Redirector to C2
https://crypt0jan.medium.com/red-team-tutorials-5-fd2f3ef6d257
https://coffeegist.com/security/resilient-red-team-https-redirection-using-nginx/