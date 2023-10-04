# DockerCon 2023 Getting Started Workshop

This repository contains my answers for the various challenges I completed during the Getting Started with Docker Workshop

## Pre-Requisites

You need Docker Desktop installed and the following extension
 [Click this link](https://open.docker.com/extensions/marketplace?extensionId=mikesir87/dockercon23-extension&tag=latest)

## Completing the Challenges

### Running Containers
In this part, the main goal is to understand basic docker commands

1) docker run --name hello-world hello-world
2) docker run -it --name timer -rm ubuntu sleep 30
3) docker run -d -p 80:8080 --name nginx nginx:alpine
4) docker run -d --label training.app=mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=app mysql
5) docker run -d -p 5050:5000 --label training.app=cats --health-cmd="curl -f http://localhost:5000/healthcheck" --health-interval=5s mikesir87/cats:1.0

Main takeways:

* image name often comes last and the : allows you to pick specific image versions
* docker -> all docker commands starts with docker(!)
* run -> starts/runs a docker image
* --name -> sets the name of the container to be created
* -it -> iteractive mode; after running it, your terminal is positioned inside the container
* -d -> detached mode; terminal is not bound to the container
* -p -> X:Y;exposes port X on the host port Y
* --label -> key-pair values
* -e -> setts env variables 
* --health-cmd -> checks availability of the application
* --heath-interval -> how often should it be health-cheacked

### Working with Volumes
Here we can learn about how the command -v works

1) docker volume create database-files 
docker volume create volume1
2) docker run -d --label training.app=persistent-db -v mysql-files:/var/lib/mysql mysql:latest
3) docker run -it --name init-db -e MYSQL_DATABASE=db -e MYSQL_ROOT_PASSWORD=secret -v "$(pwd)/database-initialization:/docker-entrypoint-initdb.d" mysql
4) docker run -it -v "$(pwd)/dev-express:/app" -p 3000:3000 -w /app --name node-app --rm node:lts-alpine yarn dev

Main takeaways:
* The default command upon initializing a container is replaced by whatever is after the image name
* -v (volume) -> bind mount a volume; basically copies content from one folder to another one inside the container
* $(pwd) -> specify the path to the dev-express folder relative to current working directory
* -w -> sets container working directory

### Building images
This section covers Dockerfile usage, therefore most of the answers are inside the Dockerfiles

1) docker build -t first-build .
2) docker build -t simple-python-webapp .
3) docker build -t nonroot-node-app
4) docker build -t multi-stage-react-app

Key takeways:

* Dockerfile is basically a file that configures multiple commands that will be executed in order
* To create another stage, use FROM img:version AS stageName
* --chown -> sets file/folder owner
* Most used instructions: FROM, WORKDIR, COPY, RUN, EXPOSE, CMD

### Multi-service applications 
Get in touch with docker compose 

1) docker network create getting-started
2) docker run --name networked-db -e MYSQL_ROOT_PASSWORD=pwd -e MYSQL_DATABASE=db --network getting-started --network-alias db mysql
3) docker network create wordpress

docker run -d --network wordpress --network-alias db -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=wordpress mysql

docker run -d --network wordpress -e WORDPRESS_DB_HOST=db -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=secret wordpress:6-apache
4) see multi-deploy-compose folder 

docker compose up

5) see compose-project folder

while trying to target the dev stage Ive encountered some issues that this question on stackoverflow helped solve
https://stackoverflow.com/questions/69042916/docker-compose-does-not-start-the-specified-stage

Basically what I had to do was to run docker build before docker compose up, after that I managed to validate my answer and finish the workshop.

Key takeaways:

* Docker commands can be translated to compose files
* Use "target" section to pick between Dockerfile stages
* --network -> place container in specific network so it can communicate with other apps
* Each image have their own env variable and should be checked in their own docs
* Ports and volumes are supposed to be represented as lists
* Add network section to an app's compose file to mimic -network parameter


## Need help?

- View the [Slides](./Slides.pdf) to refresh your memory
- Join the [Docker Community Slack](https://dockr.ly/comm-slack) and hop into the `#dockercon23-getting-started-workshop` channel
