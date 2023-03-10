#base on https://github.com/sonnx91/env-develop
# Environment built with Docker Compose

This is a basic environment built using Docker Compose. It consists following:

| service | version | Port |
|---------|---------|-----|
| Nginx | latest | 80, 443 |
| Nodejs | latest | |
| Yarn | latest | |
| PHP | 5.6.X, 7.1.X, 7.4.X | 9056, 9071, 9074 |
| MariaDb | latest | 3306 |
| Mongodb | 4.0 | 27017, 37017 |
| Redis | latest | 6379 |
| ElasticSearch | 6.8.2 | 9200, 9300 |
| Supervisor | latest | |
| RabbitMQ | latest | 5672, 15672 |

> Note: As of now, we have 3 different port for different PHP versions. Use appropriate port as per your php version need: 9056, 9071, 9074

## Requirements
- Install Docker >= 18.06.1-ce
- Install Docker compose >= 1.22.0

Before anything, you need to make sure you have Docker properly setup in your environment. For that, refer to the official documentation for both Docker and Docker Compose.

## Usage
You are up and running in three simple steps:
```sh
$ git clone https://github.com/sonnx91/env-develop.git services_develop
$ cd services_develop
$ bash/setup.sh
```

#### Nginx
- Create config nginx in folder **nginx/conf.d** with name **example.conf**

```nginx
server {
    listen [::]:80;
    listen 80;

    # The host name to respond to
    server_name example.local www.example.local;

    # Path for static files
    root /var/www;

    # Custom error pages
    include h5bp/errors/custom_errors.conf;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    #
    # change filename if use PHP version other
    # php5.conf or php71.conf or php74.conf
    #
    include php74.conf;

    # Include the basic h5bp config set
    include h5bp/basic.conf;
}
```

#### add Host

```sh
$ echo '127.0.0.1 example.local www.example.local' >> /etc/hosts
```

Services is now ready!! You can access it via http://example.local

## Tools
- Adminer
- phpMyAdmin
- phpRedisAdmin
- Rockmongo

## Docker

**Build and start docker compose**
```sh
$ cd <folder-docker>
$ docker-compose up --build
```

**Run with Docker Compose**
```sh
$ docker-compose up -d
```

**List container run**
```sh
$ docker ps
```

**Attach container to debug**
```sh
$ docker exec -it <container_id> /bin/bash
```

**Install and Using Nodejs in container nginx**

```sh
$ docker exec <container_nginx_id> sh /docker-nodejs.sh
```
# docker-setup
