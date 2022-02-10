# Docker Nextcloud
## Directory Structure
```
│  .gitignore
│  docker-compose.yml
│  README.md
│
└─dockerfiles
    ├─db
    │  └─sqls
    ├─nextcloud
    │  │  db.env
    │  │
    │  └─html
    ├─nginx
    │  ├─conf.d
    │  │      default.conf
    │  │
    │  └─ssl_certs
    │          {cert_name}.key
    │          {cert_name}.pem
    │
    └─redis-catch
```
## Sitting
- docker-compose.yml
```
services:
    db:
        environment:
            - MYSQL_ROOT_PASSWORD={password}
    
    nextcloud:
        volumes:
            - {data_dir}:/var/www/html/data
        enviroment:
            - NEXTCLOUD_TRUSTED_DOMAINS={domain}
    
    nginx:
        volumes:
            - {data_dir}:/var/www/html/data
```

- dockerfiles/nginx/conf.d/default.conf
```
server {
    listen 80;
    listen [::]:80;
    server_name  {domain};
    # enforce https
    return 301 https://$server_name:443$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name {domain};

    ssl_certificate /etc/nginx/ssl_certs/{cert_name}.pem;
    ssl_certificate_key /etc/nginx/ssl_certs/{cert_name}.key;
}
```

- dockerfiles/nextcloud/db.env
```
MYSQL_DATABASE={nextcloud_database_name}
MYSQL_USER={nextcloud_db_username}
MYSQL_PASSWORD={nextcloud_db_user_password}
```