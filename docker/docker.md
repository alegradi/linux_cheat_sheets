# Docker

## Basics
docker ps  (to show all currently running containers)  
docker ps -a  (to show docker process list and history)  
docker search "container_name"  (for example docker search centos)  
docker pull "container_name"  (to download the container)  
docker run docker.io/"container_name"  (to run a container downloaded from dockerhub)  
docker run -it "container_name"  (to run it with an interactive shell)  

## Image management
docker images == docker image list  
docker image ls  (to show currently downloaded images)
docker rmi "id_or_name"  (to remove a docker image, there cannot be any containers running based on that image)
docker images -a  (to show all images)  
docker images --filter "dangling=true"  (to show only dangling images)  
docker image prune  (to remove dangling images)  

## Container management
docker container ls -a  (to show all containers)
docker container prune  (to delete dangling containers)
docker stop "id_or_name"  (to stop an instance)  
docker rm "id_or_name"  (to remove that docker container)  
docker inspect "container_name"  (to verify the environment variables set on that docker container)  
docker run "image" sleep 100  (to have the instance run and sleep for 100 seconds before terminating)  
docker exec "container_name_or_id" "command"  (to run a command against a container)
    for example: docker exec dictionary_bulvark cat /etc/hosts  

docker run -d "image"  (to start docker container in detached mode)  
docker attach "id"  (to reattach a container to the terminal)  
docker run -i "image"  (interactive mode, the instance accepts from standard input)  
docker run -it "image"  (interactive mode with terminal attached)  
docker run -it --rm "image"  (to run container with removing once stopped, wont show with docker ps -a)
docker run -p 80:5000 "image"  (Port mapping, map port 80 of the host to port 5000 of the container)  
docker run -v /opt/datadir:/var/lib/mysql mysql  (to mount the mysql container's /var/lib/mysql on the host's /opt/datadir)
docker logs "container_name"  (to see logs of a container if running in detached mode)  
docker run -e RANDOM_VARIABLE=VALUE "image_name_or_id"  (to pass in environment variables into the container when running)  
docker run --entrypoint sleep "image_name" 10  (to overwrite the default entry point and specify a value to it)  

## How to create an image
1, Determine steps
 - OS
 - Update packages
 - Install dependencies
 - Install python dependencies with pip
 - Copy source code to /opt
 - Run webserver

### Example
DockerFile contents:

    FROM Ubuntu
        RUN apt-get update
        RUN apt-get install python
        RUN pip install flask
        RUN pip install flask-mysql
        COPY . /opt/source-code
        ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run

### Image build
        docker build DockerFile -t name/app-name  (like geerlingguy/redis)
        docker push name/app-name  (to make is available on dockerhub)
