# The Docker setup for Laravel using NGINX, PHP7-FPM and MySQL

## Requires

- Docker Engine (https://docs.docker.com/engine/installation/)
- Docker Compose (https://docs.docker.com/compose/install/)
- NGINX container (`docker pull NGINX`)
- PHP container (`docker pull php`)
- MySQL container (`docker pull mysql`)

## Instructions

1. Checkout the repository
2. Run `docker-compose up`
3. Go to your Laravel root folder and give write permission to `bootstrap/cache` and `storage` directories (`sudo chmod -R 777 bootstrap/cache storage`)
4. Navigate to [http://localhost/](http://localhost/)

## Setup

Here's an overview of the setup, this repository contains the default Laravel 5.3 install other than the files listed below.

`/deploy` This is to keep all of the deployment configuration files together and organised.

`docker-compose.yml`
This file lets us tell Docker how to build the environment. `docker-compose up` is called, it will read this file and build the necessary containers as well as configure things like networking and volumes.

`deploy/app.docker`
This is the Dockerfile for our app container. It just extends the PHP base image provided by Docker and installs some extra extensions that Laravel needs (mcrypt and mysql).

`deploy/web.docker`
This is the Dockerfile for our web container. It extends the [NGINX base image](https://hub.docker.com/_/nhinx/) provided by Docker, and just adds an NGINX config file so our web service knows how to handle requests.

`deploy/vhost.conf`
This is the NGINX config file thats added to our web container. It's a pretty standard host configuration that proxy's PHP requests to our app container. You'll notice that it communicates with the app container via address `app:9000`. The `app` name is what we named our service and linked to in `docker-compose.yml`, so Docker will know we mean that container and route the request appropriately.