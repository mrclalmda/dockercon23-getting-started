# DockerCon 2023 Getting Started Workshop

This repository contains my answers for the various challenges I completed during the Gettign Started with Docker Workshop

## Pre-requisites

You need Docker Desktop installed and the following extension
 [Click this link](https://open.docker.com/extensions/marketplace?extensionId=mikesir87/dockercon23-extension&tag=latest)

## Completing the challenges

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


## Need help?

- View the [Slides](./Slides.pdf) to refresh your memory
- Join the [Docker Community Slack](https://dockr.ly/comm-slack) and hop into the `#dockercon23-getting-started-workshop` channel
