## Docker

Cheatsheet - https://github.com/wsargent/docker-cheat-sheet

Docker (Machines)

List all Machines

    docker-machine ls

Display machine environment, including IP address and port:

    docker-machine env
    
SSH into your docker instance

    docker-machine ssh
    
See: https://docs.docker.com/machine/reference/
    
Docker (Containers)

Build a container from a Dockerfile (this will then be displayed in the images list once run).

    docker build --no-cache -t <nameofimage> .

List all images

    docker images

Run a container

    docker run --name <servicename> <imagename>

some options around this (-d for daemon, -i for interative, -t for tty options).

n.b. Your docker container will exit if a process is completed.  You need to have a service running.  Alternatively, you can invoke (docker run ... /bin/bash) to have it keep a shell open for you to run subsequent commands.

Stop a container

    docker stop <name>

Remove a container

    docker rm <name>
    
Show running containers

    docker ps
    
Get all info (including IP)

    docker inspect <name>
    docker inspect <name> | grep IPAddress
    
Show stats

    docker stats <name>
    
Run a command in a container

    docker exec <command> <name>

## Using Images (Templates)

Uses the machine: default (in Virtualbox)

Downloads the hello-world image, creates a container and ran the image

    docker run hello-world

Adventure:

    docker run -it ubuntu bash

## Download and run an image. https://docs.docker.com/mac/step_two/

Docker Hub - https://hub.docker.com/?utm_source=getting_started_guide&utm_medium=embedded_MacOSX&utm_campaign=find_whalesay

    https://hub.docker.com/

Run whalesay:

    docker run docker/whalesay cowsay budsy-boo-bear

Display ALL the images:

    docker images

## Build Your Own Image - https://docs.docker.com/mac/step_four/

See the following for command usage - https://github.com/wsargent/docker-cheat-sheet#dockerfile

Simple as taking an existing base image and customising a bit to add fortunes:

    FROM docker/whalesay:latest
    RUN apt-get -y update && apt-get install -y fortunes
    CMD /usr/games/fortune -a | cowsay

then build with the following to build a Docker image (view with docker images):

    docker build -t my-whale .

Finally, let's run it:

    docker run my-whale

Next steps, Dockerhub -> Create Repository.  Tag your docker instance with:

    docker tag <imageid> <accountname>/<dockername>:<versionlabel>
    docker tag 39c8a1513c69 jph98/my-whale:latest
  
Login and push:

    docker login --username <username> --email <email>
    docker push jph98/my-whale
  
Remove with:

    docker rmi -f <imageid>
  
Download with:

     docker pull yourusername/docker-whale
   
You can search for a particular image on the command line with:

    docker search java:java8
  
Install Docker toolbox - https://docs.docker.com/installation/mac/
Products - http://www.docker.com/products/overview
Setup an automated build - https://docs.docker.com/docker-hub/

## Using Docker Images For Processing

If we're using Docker images for processing we have a number of options once finished processing files/exporting data etc..

1. Output database records to a file on the container and copy that back to the host from the image
2. Upload the file to S3/GCP Storage from the container
3. Stream the database records to BigQuery
4. Stream the database records to an SQS queue Or lambda

Run a container called "testdock" with a volume attached for the image "testdock:01.":

    docker run -it --name testdock -v /opt/pipeline testdock:0.1

n.b. You can pass multiple volumes

i - follow this interactively.  Alternatively you can detach.
p - expose a port
v - specify the volume
t - In foreground mode, allocate a pseudo tty for output (or no output)

## Volumes

There are two ways to deal with volumes:
* -v which provides a combined model
* -mount which provides more control

When using -v the following specifies a volume with name "blah" for a target directory:

    -v blah:/opt/pipeline
    
It is created on the fly.

You can use mount as follows which mounts /source on host to /app on the target:

    --mount type=bind,source="$(pwd)"/source,target=/app
    
## Delete all the containers

    docker rm $(docker ps -a -q) -f

## Delete all volumes

    docker volume prune
