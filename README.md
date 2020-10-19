# Desafio Docker

## Dockerize (Challenge 1)

Create a .env file in the root project. Like this:

```bash
DB_HOST=la-db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=notroot

APP_HOST=la-app
APP_PORT=9000

NGINX_PORT=8001
NGINX_INTERNAL=80
```

### Run project

```bash
docker-compose up --build
```


## Golang (Challenge 2 and 3)

[Dockerhub](https://registry.hub.docker.com/repository/docker/turnes/codeeducation)

```bash
docker run turnes/codeeducation
```
Images less then 2MB
```bash
docker images
REPOSITORY                                TAG                    IMAGE ID            CREATED             SIZE
turnes/codeeducation                      latest                 eb40449020af        17 minutes ago      1.23MB
```


## [Dockerize](https://github.com/jwilder/dockerize)

For App and Nginx container you need to add dockerize in Dockerfile. In docker-compose use the option entrypoint calling dockerize.


### Dockerfile

#### Add

```dockerfile
# Dockerize
ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
```

Add some template file.
Nginx
```dockerfile
RUN rm /etc/nginx/conf.d/default.conf
COPY ./nginx.tmpl /etc/nginx/conf.d/
```

### docker-compose.yml

You need to change the entrypoint to use dockerize.

APP
```docker
entrypoint: dockerize -template .docker/app/.env:.env -wait tcp://la-db:3306 -timeout 40s docker-entrypoint.sh
```

Nginx
```docker
entrypoint: dockerize -template nginx.tmpl:nginx.conf -wait tcp://${APP_HOST}:${APP_PORT} -timeout 40s nginx -g "daemon off;"
```

## Env file

You need to create a .env file in the root project.


```bash
DB_HOST=la-db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=notroot

APP_HOST=la-app
APP_PORT=9000

NGINX_PORT=8000
NGINX_INTERNAL=80
```