
# docker installation

## Ubuntu linux

Install docker using `apt`:

    sudo apt install docker.io

With this docker can only be run as root user. Add your user to the docker usergroup to be able to run it as well.

    sudo usermod -a -G docker $USER

This will append (`-a`) `$USER` to the usergroup (`-G`) `docker`. The current usergroups can be found and checked in `/etc/group`.
It will need a re-login for the changes to take effect.

https://www.linux.com/learn/intro-to-linux/2017/11/how-install-and-use-docker-linux


### Docker handling

- create a docker build file named `Dockerfile` in the root of the project
- write the shell script equivalent of installing dependencies and the project in the dockerfile and expose a port
- build docker file from the root of the project containing the docker build file and give the container a name e.g. example

        docker build -t [container] .

- the configuration of a built docker container can be reviewed by using

        docker inspect [container]

- show all created containers

        docker image ls -a

- a built docker container can be start by using

        docker run [container]

- a built docker container can be run and accessed using the command. `-it` means interactive, `--entrypoint /bin/bash`
    opens the shell of the container giving cli access.
- enter a container at startup to look around for any setup problems. This will not start the service since the
    `entrypoint` is overwritten by executing `/bin/bash`. The container can be left by typing in `exit`.

        docker run -t --entrypoint=/bin/bash [container]

- show all running containers, get names of the running containers

        docker ps

- show the details of a running container, get the name of the container runtime via `docker ps`
    this will display the ip address under which the docker container is accessible.

        docker inspect [docker ps runtime name]

- look into a running container to e.g. check whether files from the outside have been
    properly mapped to inside the container.
    
    docker exec -it [docker ps runtime name] /bin/bash

- stop a running docker container

        docker kill [docker ps runtime name]

- mounting directories from the docker container to the outside and vice versa

        docker run -it -v /home/Chaos/DL/docker/results:/results -v /home/Chaos/DL/docker/temp:/temp -v /home/Chaos/DL/docker/config:/config gnode/gin-valid

- run docker in detached (background) mode

        docker run -d [container]

- run a container in detached mode, give it a name and stop it after its done.

        docker run -d --name [someName] [container]
        docker stop [someName]
        docker rm [someName] # remove the named container so we can create a container of the same name again

        # with the flag --rm a named container gets automatically remove when it is stopped
        docker run --rm -d --name [someName] [container]
        docker stop [someName]

        # make sure the docker container keeps running in the background even if every script has been
        # executed use the flags -dit
        docker run --rm -dit --name [someName] [container]
        docker stop [someName]

- [publish a docker container](https://ropenscilabs.github.io/r-docker-tutorial/04-Dockerhub.html)
  - create a repo on dockerhub
  - locally docker login
  - create a tag from the to be published image - ideally use tagname "latest" if you want a docker pull to be easy.

        docker tag [imageID] [dockerhubUsername]/[reponame]:[tagname]

  - push tag to dockerhub
  
        docker push [dockerhubUsername]/[reponame]:[tagname] 

- docker debugging on failing background containers:

When working with a docker container running in the background it can be hard to debug, if it fails and ends.
In this case get the logfile that is being written while the container is still up and running:

    docker inspect [docker runtime name] | grep log
    # get the logfile name and check the available logs.


## Docker cleanup
https://zaiste.net/removing_docker_containers/
https://www.calazan.com/docker-cleanup-commands/
https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes

List all exited containers

    docker ps -aq -f status=exited

Remove stopped containers

    docker ps -aq --no-trunc -f status=exited | xargs docker rm

Cleanup dangling images

    docker images -f "dangling=true" -q

Remove unused data

    docker system prune

### Cleanup pipe/script

    docker ps -aq --no-trunc -f status=exited | xargs docker rm
    docker images -f "dangling=true" -q | xargs docker rmi
    docker system prune
