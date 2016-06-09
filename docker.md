# Docker - An open platform for distributed applications
Docker requires a 64-bit installation of any popular linux Distro.

## Instalation on Ubuntu
```bash
# Update package information, ensure that APT works with the https method, and that CA certificates are installed.
$ apt-get install apt-transport-https ca-certificates
# Add the new GPG key
$ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
# Open the /etc/apt/sources.list.d/docker.list file in your favorite editor.
# Remove any existing entries.
# Add an entry for your Ubuntu operating system.
# On Ubuntu Trusty 14.04 (LTS)
# deb https://apt.dockerproject.org/repo ubuntu-trusty main
$ apt-get update
$ apt-get purge lxc-docker
$ apt-cache policy docker-engine
# Prerequisites by Ubuntu Trusty 14.04 (LTS)
$ sudo apt-get install linux-image-extra-$(uname -r)
# If you are installing on Ubuntu 14.04 or 12.04, apparmor is required. You can install it using: apt-get install apparmor
$ sudo apt-get install docker-engine
# start daemon
$ sudo service docker start
```
## Images & Containers

### creates a container from busybox image
```bash
# creates and starts a container
$ sudo docker run busybox echo Hello Docker
```
### running containers
```bash
$ sudo docker ps
# ps -a also shows stopped containers
$ sudo docker ps -a
```
### clean up old containers
```bash
$ sudo docker rm $(docker ps -aq)
```
### images are templates for docker containers
When we talk about an image it is the suspended state. When we talk about container it is a run-time instance of a docker image.
```bash
# shows all images
$ sudo docker images
# removes an image
$ sudo docker rmi
# show history of image
$ sudo docker history
```
### running container as a deamon map host:quest port naming it
```bash
$ sudo docker run -d -p 8080:8080 --name docdev celavi/javadev:1.0.0
```
### kill running container
```bash
$ sudo docker kill <container_id>
```
### checking logs of container
```bash
$ sudo docker logs -f <container_id>|<container_name>
```
### run an interactive container
```bash
$ sudo docker run -d -p 8080:8080 -t -i --name docdev celavi/javadev:1.0.0 bash
$ sudo docker attach docdev # if this was virtual machine we would ssh to machine
$ curl -I http://localhost:8080/dockerapp
```
### find the IP of container
```bash
$ sudo docker inspect <container_id>|<container_name>
$ sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container_id>
```
##  Docker Hub
The Docker Hub is a cloud-based centralized resource for container image discovery, image building, and distribution of those images.
You can search for a "ready to download and run docker image", by simply visiting the online docker hub or by using the terminal.

```bash
$ sudo docker search wordpress
$ sudo docker pull bitnami/wordpress:latest
$ sudo docker search dokuwiki
$ sudo docker pull mprasil/dokuwiki
```
## Docker Workflow

### the phusion base image
```bash
# https://github.com/phusion/baseimage-docker
# pulling
$ sudo docker run --rm -t -i phusion/baseimage:0.9.17 /sbin/my_init -- bash -l
# after adding some additional layer: apt-get install vim
# we save containers state to an image, so the state can be re-used.
```
### commiting (saving) a container state
```bash
$ sudo docker commit <container_id> phusion-base-with-vim
```
### pushing image to docker hub
```bash
# rename phusion-base-with-vim image
$ sudo docker tag <image_id> celavi/phusion-base-with-vim:1.0.0
# push to docker hub
$ sudo docker login
$ sudo docker push celavi/phusion-base-with-vim:1.0.0
```
## Dockerfile
```bash
# creates an image from Dockerfile
$ sudo docker build -t="celavi/javadev:1.0.0" .
# Oracle Java 8 for Debian jessie
# https://github.com/William-Yeh/docker-java8/blob/master/Dockerfile
```
### Example
```
# each instruction in Dockerfile commits a new layer to the image
# download an image with java 8
FROM java:8
MAINTAINER celavi
# Install maven into the image
RUN apt-get update && \
	echo "===> install maven" \
	apt-get install -y maven
# create a working directory 'code' this is where we will build and run our application
WORKDIR /code
# open port 8080
EXPOSE 8080
```
### Extend Dockerfile
```
# download an image with java 8
FROM java:8
MAINTAINER celavi
# Install maven into the image
RUN apt-get update && \
	echo "===> install maven" \
	apt-get install -y maven
# create a working directory 'code' this is where we will build and run our application
WORKDIR /code
# add the build too files and source code
COPY pom.mxl /code/pom.xml
# adding source, compile
COPY src /code/src
RUN ["mvn","package"]
# run the application
CMD ["mvn","exec:java"]
```
