# Máster en Ciencia de Datos e Ingeniería de Computadores. Prácticas de BigData y Cloud Computing. Curso 2016-2017. 

![Header](https://sites.google.com/site/manuparra/home/headerdicits.png)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) &  José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

[UGR](http://www.ugr.es) | [DICITS](http://dicits.ugr.es) | [SCI2S](http://sci2s.ugr.es) | [DECSAI](http://decsai.ugr.es)

Manuel J. Parra Royón (manuelparra@decsai.ugr.es) & José. M. Benítez Sánchez (j.m.benitez@decsai.ugr.es)

# Primeros pasos con docker

Table of Contents
=================

   * [What is docker?](#what-is-docker)
      * [What is the difference between Docker and Virtual Machines?](#what-is-the-difference-between-docker-and-virtual-machines)
         * [VIRTUAL MACHINES](#virtual-machines)
         * [Containers](#containers)
      * [Advantages of Docker](#advantages-of-docker)
   * [Starting with docker](#starting-with-docker)
   * [First container](#first-container)
      * [Docker Market or Docker Hub](#docker-market-or-docker-hub)
      * [Creating a RStudio Data Science service](#creating-a-rstudio-data-science-service)
         * [Status of your containers](#status-of-your-containers)
         * [Example using RStudio Service with RSNNS](#example-using-rstudio-service-with-rsnns)
      * [ADVANCED: A simple web server with NGINX](#advanced-a-simple-web-server-with-nginx)
         * [Status of your containers](#status-of-your-containers-1)
         * [Enter in the nginx container:](#enter-in-the-nginx-container)
         * [Docker ports redirection](#docker-ports-redirection)
   * [ Review of docker commands](#review-of-docker-commands)
      * [Show docker Images](#show-docker-images)
      * [List of launched/running docker containers:](#list-of-launchedrunning-docker-containers)
      * [Download a image of docker container:](#download-a-image-of-docker-container)
      * [Run a container](#run-a-container)
      * [Stop a container](#stop-a-container)
      * [Restarting a container](#restarting-a-container)
      * [Deleting a container or shutdown a container](#deleting-a-container-or-shutdown-a-container)
      * [Execute commands inside running container:](#execute-commands-inside-running-container)
      * [Upload image to docker::hub](#upload-image-to-dockerhub)
      * [Statistics of a container](#statistics-of-a-container)
      * [Logs of a container](#logs-of-a-container)
   * [Basic exercise with containers:](#basic-exercise-with-containers)
   * [References and more information](#references-and-more-information)


# What is docker?

Docker containers wrap a piece of software in a complete filesystem that contains everything needed to run: code, runtime, system tools, system libraries, anything that can be installed on a server. This guarantees that the software will always run the same, regardless of its environment.

## What is the difference between Docker and Virtual Machines?

Containers and virtual machines have similar resource isolation and allocation benefits, but a different architectural approach allows containers to be more portable and efficient.

### VIRTUAL MACHINES

Virtual machines include the application, the necessary binaries and libraries, and an **entire guest operating system**,  all of which can amount to tens of GBs.

![VMsDiff](https://www.docker.com/sites/default/files/VM%402x.png)

### Containers

Containers include the application and all of its dependencies, but **share the kernel with other containers, running as isolated processes in user space on the host operating system**. Docker containers are not tied to any specific infrastructure: they run on any computer, on any infrastructure, and in any cloud.

![DockersDiff](https://www.docker.com/sites/default/files/what_is_a_container.png)

## Advantages of Docker

**Rapid development**

- Stop wasting hours setting up developer environments, spinning up new instances, and making copies of production code to run locally. With Docker, you simply take copies of your live environment and run them on any new endpoint running a Docker engine.

**Work comfortably**

- The isolation capabilities of Docker containers free developers from constraints: they can use the best language and tools for their application services without worrying about causing internal tooling conflicts.

**Forget inconsistences**

- Packaging an application in a container with its configs and dependencies guarantees that the application will always work as designed in any environment: locally, on another machine, in test or production. No more worries about having to install the same configurations into different environments.

**Share your containers**

- Store, distribute, and manage Docker images in Docker Hub with your team. Image updates, changes, and history are automatically shared across your organization.

**Scale**

- Docker allows you to dynamically change your application from adding new capabilities and scaling services, to quickly changing problem areas.

- Docker containers spin up and down in seconds, making it easy to scale application services to satisfy peak customer demand, and then reduce running containers.

- Docker makes it easy to identify issues, isolate the problem container, quickly roll back to make the necessary changes, and then push the updated container into production. Isolation between containers makes these changes less disruptive than in traditional software models.

- Docker Swarm !

**Multiplataform**

- Docker runs on Windows, Mac and Linux.

![DockerGrow](https://www.docker.com/sites/default/files/docker-pulls_0.png)


# Starting with docker

Log and connect to our hadoop cluster system with:
Firstly try to access to the hadoop ugr server:

```
ssh manuparra@hadoop....
```

Change `manuparra` with your `user login`: 

```
ssh mdatXXXXXXX@hadoop....
```

If you don't kwnow your login, go to [Login](./README.md)

Use your previously changed password, when asked.

First of all, check that you have access to the docker system, try this command:

```
docker run hello-world
```

And it will return a message where it shows that your installation appears to be working correctly and you are allow use it.

# First container

To create a new container in docker, it can be done in two ways: 

- on the one hand by doing it by creating a **docker file** [see more information here](https://docs.docker.com/engine/getstarted/step_four/) or 

- by downloading / using a container that is already created by other users.

Second option is faster and less complex (we will use docker in this way).

## Docker Market or Docker Hub

Go to [docker hub](https://hub.docker.com/).

Containers already created available to be used are stored in a kind of container market at https://hub.docker.com/. *Virtually anything you think will already be dockerized*. 

But you can also build your own container with everything you need. For example a container having your complete application with all its dependencies, mixing i.e. php, mysql, nginx, R, spark, hadoop, etc. on the same container or on different containers.

## Creating a RStudio Data Science service

The first thing we need is to download the docker image from RStudio (on docker hub is: rocker), for them we check whether or not the image is in the list of available images:


```
docker images
```

If the image is not found locally, Docker will pull it from Docker Hub and you will use it:

```
docker pull rocker/rstudio
```

It will download the image of RStudio container. 

And now you can see if image is on images repository using:

```
docker images
```

```
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
rocker/rstudio      latest              6a0886126390        13 hours ago        989.6 MB
nginx               latest              00bba88663ff        8 days ago          181.8 MB...
```

Run the container, using the next syntax:

```
docker run -d -p <yourport>:<containerport> --name <mynameofcontainer> <container>
```

Wait, we need a few minutes to understand PORT assignment:

![PortAsignmentRstudio](https://sites.google.com/site/manuparra/home/rstudio.png)

About the options:

``-d Run container in background and print container ID``

``-p Publish a container's port(s) to the host``

``--name Name of your contaniner i.e. 'containerofmanuparra'``

``<container> This is the container that will be executed`` 

So, before execute the next command we need:

- One of your EXTERNAL PORT assigned (Look [here](README.md#dockerports))
- Rstudio INTERNAL PORT (default: 8787)
- The name of your "RStudio service"

** Change test_rstudio to rstudio_myname**

```
docker run -d -p <yourassignedport>:8787 --name test_rstudio rocker/rstudio
```

In ``<yourassignedport>`` write your individually assigned port. See your ports [here](README.md#dockerports).

To check if your container is runnig, show the status of all your container:

### Status of your containers

```
docker ps
```

And it returns:

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
6ff2fa0c41f3        rocker/rstudio      "/init"             4 seconds ago       Up 3 seconds        0.0.0.0:16001->8787/tcp   serene_bohr ...
```

Using:

```
docker ps -a
```

Will returns containers history (all):

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                            NAMES
52ad2efb9fff        nginx               "nginx -g 'daemon off"   12 minutes ago      Up 12 minutes       443/tcp, 0.0.0.0:14000->80/tcp   testnginx
...
```

**IMPORTANT:** 

Where ``container ID`` is the unique ID of your Container. You can use Container ID or NAMES to refer to your container. ``IMAGE`` is the name of the container image. ``PORTS`` show what is the correspondence of the ports between server and docker container.


And now, go to your browser and write:

```
http://hadoop.ugr.es:<yourassignedport>/
```

Use default credentials to login Your Rstudio service: 

```
usename: rstudio 
passwd: rstudio 
```

### Example using RStudio Service with RSNNS

Try this code:

- Install fpp package
- Install rsnns package


```
######################################################################
####FEED FORWARD NEURAL NETWORK - RSNNS PACKAGE#######################
######################################################################

# RSNNS no funciona con valores perdidos NA, luego para entrenar la red neuronal
# habría que imputar o no entrenar los valores lageados con NA

library(fpp)
library(RSNNS)
library(quantmod)

plot(sunspotarea)

#sunspotarea <- ts(scale(sunspotarea[1:length(sunspotarea)]),start=c(start(sunspotarea)[1],start(sunspotarea)[2]),end=c(end(sunspotarea)[1],end(sunspotarea)[2]))
#sunspotarea <- (sunspotarea-min(sunspotarea))/(max(sunspotarea)-min(sunspotarea))
sunspotarea <- ts(normalizeData(sunspotarea[1:length(sunspotarea)]),start=c(start(sunspotarea)[1],start(sunspotarea)[2]),end=c(end(sunspotarea)[1],end(sunspotarea)[2]))

plot(sunspotarea)

# ar(sunspotarea)$order   # nos dice de orden 9, luego 9 rezagos

dat <- data.frame(sunspotarea[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],1)[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],2)[10:length(sunspotarea)],Lag(sunspotarea[1:length(sunspotarea)],3)[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],4)[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],5)[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],6)[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],7)[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],8)[10:length(sunspotarea)], Lag(sunspotarea[1:length(sunspotarea)],9)[10:length(sunspotarea)]) # create with lagged values
colnames(dat) <- c("y",paste0("Lag.", 1:(ncol(dat)-1)))
head(dat)

fit <- mlp(dat[2:length(dat)],dat[1],size=5,linOut=T)

ps <- predict(fit, dat[2:length(dat)])

# Examine results
plot(time(sunspotarea)[1:length(sunspotarea)],sunspotarea[1:length(sunspotarea)],type="l",col = 2)
lines(time(sunspotarea)[10:length(sunspotarea)],ps, col=3)


# Prediction
pred <- myPrediction(sunspotarea,fit,9,20,colnames(dat[2:length(dat)]),"rsnns")


# Examine prediction
plot(time(sunspotarea)[1:length(sunspotarea)],sunspotarea[1:length(sunspotarea)],type="l",xlim=c(min(time(sunspotarea)),max(max(time(sunspotarea))+20)),ylim=c(min(sunspotarea,pred),max(sunspotarea,pred)))
lines(time(sunspotarea)[10:length(sunspotarea)],ps, col="red")
lines(seq(max(time(sunspotarea)),max(time(sunspotarea))+20,length=20), pred, col="blue")

```


## ADVANCED: A simple web server with NGINX

The first thing we need is to download the docker image from nginx, for them we check whether or not the image is in the list of available images:

```
docker images
```

If the image is not found locally, Docker will pull it from Docker Hub and you will use it:

```
docker pull nginx
```

It will download the image of nginx container. 

And now you can see if image is on images repository using:

```
docker images
```

```
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
docker.io/nginx                   latest              abf312888d13        12 days ago         181.5 MB
...
```

Run the container, using the next syntax:

```
docker run -d -p <yourport>:<containerport> --name <mynameofcontainer> <container>
```

Options:

``-d Run container in background and print container ID``

``-p Publish a container's port(s) to the host``

``--name Name of your contaniner i.e. 'containerofmanuparra'``

``<container> This is the container that will be executed`` 

So, we execute:

** Change testnginx  to NGINX_YOURNAME**


```
docker run -d -p <yourport>:80 --name testnginx nginx
```


In ``<yourport>`` write your individually assigned port. See your ports [here](https://github.com/manuparra/MasterDatCom_BDCC_Practice#dockerports).

To check if your container is runnig, show the status of all your container:

### Status of your containers

```
docker ps
```

And it returns:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                            NAMES
52ad2efb9fff        nginx               "nginx -g 'daemon off"   12 minutes ago      Up 12 minutes       443/tcp, 0.0.0.0:14000->80/tcp   testnginx
...
```

Using:

```
docker ps -a
```

Will returns containers history (all):

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                            NAMES
52ad2efb9fff        nginx               "nginx -g 'daemon off"   12 minutes ago      Up 12 minutes       443/tcp, 0.0.0.0:14000->80/tcp   testnginx
...
```

Where ``container ID`` is the unique ID of your Container. You can use Container ID or NAMES to refer to your container. ``IMAGE`` is the name of the container image. ``PORTS`` show what is the correspondence of the ports between server and docker container.


And now, go to your browser and write:

```
http://hadoop.ugr.es:<yourport>/
```

![nginxDocker](https://sites.google.com/site/manuparra/home/docker_nginx.png)

### Enter in the nginx container:

```
docker exec -i -t testnginx /bin/bash
```

With this command your are inside of the container and you can modify things, for instance change the main website exposed by NGINX:

```
vim /usr/share/nginx/html/index.html 
```

ERROR: command ``vi`` is not in container. 

***Why***? -> Container has been built with minimal software! ... , so you need to install it:

```
apt-get install vim
```

and try again:

```
vim /usr/share/nginx/html/index.html 
```

Edit the file and change something ``<H1>Hello Your Name</H1>``. Save and close and try again in your browser: 

```
http://hahoop.ugr.es:<yourport>/
```

![dockerchanged](https://sites.google.com/site/manuparra/home/docker_changed_nginx.png)

### Docker ports redirection

This diagram shows the port equivalence between the docker containers and the server ports. As you can see, the containers can run on the same ports, as they are independent of each other.

![DockerPorts](https://sites.google.com/site/manuparra/home/portasig.jpg)

In addition each container can export several ports:


```
docker run -d -p 12000:80 -p 12001:81   --name <name> <container>
```


# Review of docker commands


## Show docker Images

With:

```
docker images
```

The default docker images will show all top level images, their repository and tags, and their size, and it will return:

```
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
docker.io/tomcat                  latest              c6cfe59eb987        3 weeks ago         356.9 MB
docker.io/osixia/openldap         1.1.7               7043188ce9b7        4 weeks ago         223 MB
...
```

## List of launched/running docker containers:

``docker ps``


Show all docker containers running and details about execution, ports and status.

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                            NAMES
52ad2efb9fff        nginx               "nginx -g 'daemon off"   About an hour ago   Up About an hour    443/tcp, 0.0.0.0:14000->80/tcp   testnginx
```


## Download a image of docker container:

``docker pull``

Download the image container selected. You can go to https://hub.docker.com/ and search for 'dockerized' images from other users.

```
docker pull <nameofcontainer>
```

```
docker pull jetbrainshost/telegram-bot
```

It will download a telegram bot named: jetbrainshost/telegram-bot


## Run a container

``docker run``

Run a docker image. It allows to assign port in/out, name, external folders, etc. 

```
docker run -d -p <yourport>:80 --name <name> <container>
```

```
docker run -d -p 8080:80 --name testnginx nginx
```

It will run nginx at 8080 external port ant connect it with internal container port 80. The name of container will be testnginx.

Run a container allows multiple options to improve docker performance:

```
      --add-host value              Add a custom host-to-IP mapping (host:ip) (default [])
  -a, --attach value                Attach to STDIN, STDOUT or STDERR (default [])
      --blkio-weight value          Block IO (relative weight), between 10 and 1000
      --blkio-weight-device value   Block IO weight (relative device weight) (default [])
      --cap-add value               Add Linux capabilities (default [])
      --cap-drop value              Drop Linux capabilities (default [])
      --cgroup-parent string        Optional parent cgroup for the container
      --cidfile string              Write the container ID to the file
      --cpu-percent int             CPU percent (Windows only)
      --cpu-period int              Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int               Limit CPU CFS (Completely Fair Scheduler) quota
  -c, --cpu-shares int              CPU shares (relative weight)
      --cpuset-cpus string          CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string          MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                      Run container in background and print container ID
      --detach-keys string          Override the key sequence for detaching a container
      --device value                Add a host device to the container (default [])
      --device-read-bps value       Limit read rate (bytes per second) from a device (default [])
      --device-read-iops value      Limit read rate (IO per second) from a device (default [])
      --device-write-bps value      Limit write rate (bytes per second) to a device (default [])
      --device-write-iops value     Limit write rate (IO per second) to a device (default [])
      --disable-content-trust       Skip image verification (default true)
      --dns value                   Set custom DNS servers (default [])
      --dns-opt value               Set DNS options (default [])
      --dns-search value            Set custom DNS search domains (default [])
      --entrypoint string           Overwrite the default ENTRYPOINT of the image
  -e, --env value                   Set environment variables (default [])
      --env-file value              Read in a file of environment variables (default [])
      --expose value                Expose a port or a range of ports (default [])
      --group-add value             Add additional groups to join (default [])
      --health-cmd string           Command to run to check health
      --health-interval duration    Time between running the check
      --health-retries int          Consecutive failures needed to report unhealthy
      --health-timeout duration     Maximum time to allow one check to run
      --help                        Print usage
  -h, --hostname string             Container host name
  -i, --interactive                 Keep STDIN open even if not attached
      --io-maxbandwidth string      Maximum IO bandwidth limit for the system drive (Windows only)
                                    (Windows only). The format is `<number><unit>`.
                                    Unit is optional and can be `b` (bytes per second),
                                    `k` (kilobytes per second), `m` (megabytes per second),
                                    or `g` (gigabytes per second). If you omit the unit,
                                    the system uses bytes per second.
                                    --io-maxbandwidth and --io-maxiops are mutually exclusive options.
      --io-maxiops uint             Maximum IOps limit for the system drive (Windows only)
      --ip string                   Container IPv4 address (e.g. 172.30.100.104)
      --ip6 string                  Container IPv6 address (e.g. 2001:db8::33)
      --ipc string                  IPC namespace to use
      --isolation string            Container isolation technology
      --kernel-memory string        Kernel memory limit
  -l, --label value                 Set meta data on a container (default [])
      --label-file value            Read in a line delimited file of labels (default [])
      --link value                  Add link to another container (default [])
      --link-local-ip value         Container IPv4/IPv6 link-local addresses (default [])
      --log-driver string           Logging driver for the container
      --log-opt value               Log driver options (default [])
      --mac-address string          Container MAC address (e.g. 92:d0:c6:0a:29:33)
  -m, --memory string               Memory limit
      --memory-reservation string   Memory soft limit
      --memory-swap string          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int       Tune container memory swappiness (0 to 100) (default -1).
      --name string                 Assign a name to the container
      --network-alias value         Add network-scoped alias for the container (default [])
      --network string              Connect a container to a network
                                    'bridge': create a network stack on the default Docker bridge
                                    'none': no networking
                                    'container:<name|id>': reuse another container's network stack
                                    'host': use the Docker host network stack
                                    '<network-name>|<network-id>': connect to a user-defined network
      --no-healthcheck              Disable any container-specified HEALTHCHECK
      --oom-kill-disable            Disable OOM Killer
      --oom-score-adj int           Tune host's OOM preferences (-1000 to 1000)
      --pid string                  PID namespace to use
      --pids-limit int              Tune container pids limit (set -1 for unlimited)
      --privileged                  Give extended privileges to this container
  -p, --publish value               Publish a container's port(s) to the host (default [])
  -P, --publish-all                 Publish all exposed ports to random ports
      --read-only                   Mount the container's root filesystem as read only
      --restart string              Restart policy to apply when a container exits (default "no")
                                    Possible values are : no, on-failure[:max-retry], always, unless-stopped
      --rm                          Automatically remove the container when it exits
      --runtime string              Runtime to use for this container
      --security-opt value          Security Options (default [])
      --shm-size string             Size of /dev/shm, default value is 64MB.
                                    The format is `<number><unit>`. `number` must be greater than `0`.
                                    Unit is optional and can be `b` (bytes), `k` (kilobytes), `m` (megabytes),
                                    or `g` (gigabytes). If you omit the unit, the system uses bytes.
      --sig-proxy                   Proxy received signals to the process (default true)
      --stop-signal string          Signal to stop a container, SIGTERM by default (default "SIGTERM")
      --storage-opt value           Storage driver options for the container (default [])
      --sysctl value                Sysctl options (default map[])
      --tmpfs value                 Mount a tmpfs directory (default [])
  -t, --tty                         Allocate a pseudo-TTY
      --ulimit value                Ulimit options (default [])
  -u, --user string                 Username or UID (format: <name|uid>[:<group|gid>])
      --userns string               User namespace to use
                                    'host': Use the Docker host user namespace
                                    '': Use the Docker daemon user namespace specified by `--userns-remap` option.
      --uts string                  UTS namespace to use
  -v, --volume value                Bind mount a volume (default []). The format
                                    is `[host-src:]container-dest[:<options>]`.
                                    The comma-delimited `options` are [rw|ro],
                                    [z|Z], [[r]shared|[r]slave|[r]private], and
                                    [nocopy]. The 'host-src' is an absolute path
                                    or a name value.
      --volume-driver string        Optional volume driver for the container
      --volumes-from value          Mount volumes from the specified container(s) (default [])
  -w, --workdir string              Working directory inside the container
```


We comment on several of the most relevant options to optimize the operation of the container: cpu-limit, memory-limit, external storage, ...


## Stop a container

You can stop a container using

```
docker stop <nameofcontainer or container ID>
```

```
docker stop testnginx
```


## Restarting a container

You can restart a container using:

```
docker restart <nameofcontainer or container ID>
```

```
docker restart testnginx
```

## Deleting a container or shutdown a container

If the lifecycle of your container has finished and you want remove it:

```
docker rm <nameofcontainer or container ID>
```

```
docker rm testnginx
```

If the container is running, first try ``stop `` and then ``rm``, but if you cant stop it, force remove:

```
docker rm --force <nameofcontainer or container ID>
```

```
docker rm --force testnginx
```

## Execute commands inside running container:

If you need modify files or add something inside container:

```
docker exec -i -t <nameofcontainer or ID> /bin/bash
```

```
docker exec -i -t testnginx /bin/bash
```

It will open a shell (bash) into the container to manage internal container.


## Upload image to docker::hub

If you have created a container with an application or service already prepared you can upload it to the application repository of DockerHub.

```
docker push manuparra/myapp_dockerized
```

Previously you must have registered on the dockerhub platform.

## Statistics of a container

Display a live stream of container resource usage statistics. It is very useful to know what is the performance of you services or application deployed with docker.

```
docker stats testnginx
```

```
CONTAINER           CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O
testnginx           0.00%               75.51 MB / 67.32 GB   0.11%               16.53 MB / 393 kB   94.21 kB / 0 B
```

## Logs of a container

To view the logs for a container it’s as simple as running just one command:

```
docker logs testnginx
```

```
90.170.152.124 - - [11/Dec/2016:12:03:08 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.98 Safari/537.36" "-"
```


# Basic exercise with containers:
   


# References and more information

- Definition of docker: https://www.docker.com/what-docker
- Docker Hub: https://hub.docker.com/
- First steps with Docker: https://docs.docker.com/engine/getstarted/
- Running `Hello World`: https://docs.docker.com/engine/tutorials/dockerizing/
- Free EBook about Docker:  https://goto.docker.com/docker-for-the-virtualization-admin.html
