- [Hello-world](#hello-world)
- [Docker commands](#docker-commands)
    - [Docker run](#docker-run)
    - [Docker create](#docker-create)
    - [Docker start](#docker-start)
    - [Docker start to restart](#docker-start-to-restart)
    - [Cleaning up containers with `prune`](#cleaning-up-containers-with-prune)
    - [Getting logs from a container using docker logs](#getting-logs-from-a-container-using-docker-logs)
    - [Stopping a docker container with stop](#stopping-a-docker-container-with-stop)
  - [Killing all containers](#killing-all-containers)
    - [The -it flag (-i flag and -t flag)](#the--it-flag--i-flag-and--t-flag)
  - [Executing additional commands in a container](#executing-additional-commands-in-a-container)
  - [Opening up a shell/terminal in a container](#opening-up-a-shellterminal-in-a-container)
- [Section 3: Creating Custom images](#section-3-creating-custom-images)
  - [Steps to create a custom image](#steps-to-create-a-custom-image)
- [Dockerfile](#dockerfile)
- [Docker Build .](#docker-build-)
  - [File flag](#file-flag)
    - [Building from cache ( docker memoizing with image generation)](#building-from-cache--docker-memoizing-with-image-generation)
- [Tagging an image](#tagging-an-image)
- [Commit: Creating an image from a container](#commit-creating-an-image-from-a-container)
- [Copy command in Dockerfile](#copy-command-in-dockerfile)
- [Publish flag: Port mapping](#publish-flag-port-mapping)
- [Setting Working directory using WORKDIR](#setting-working-directory-using-workdir)
- [Docker Compose](#docker-compose)
  - [docker-compose down](#docker-compose-down)
  - [docker-system ps](#docker-system-ps) - [Docker volume](#docker-volume)
- [Production grade workflow with docker](#production-grade-workflow-with-docker)
  - [docker files](#docker-files)
  - [Docker commands](#docker-commands-1)

## Container isolation

Containers are isolated from each other using linux's namespaces.

# Hello-world

```bash
docker run hello-world

```

To generate the message from hello-world image, Docker took the following steps:

1.  The Docker client contacted the Docker daemon.
2.  The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
3.  The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
4.  The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

# Docker commands

1. Process status

```bash
docker ps
```

Lists all running containers

1.1 Process status all

```bash
docker ps --all
```

Lists history of ran docker containers

### Docker run

Docker run = docker create and docker start

-d flad runs it in the background.

### Docker create

Used to create a container that outputs the id of the container

```bash
docker create hello-world
//stdout = 6a8ab8afcff30812c4977f97588ec83f56f868ecf0177ff6b9a4560585a42be2
```

### Docker start

Used to start a docker container
It takes in the id of the container to start

```bash
docker start 6a8ab8afcff30812c4977f97588ec83f56f868ecf0177ff6b9a4560585a42be2
// logs nothing as stdout is not attached
```

By default, running docker start does not attach stdout and stderr to the container. So we do not get any output that the container sends out. This is another difference between docker run and docker start

To attach stdout and stderr to the container

```bash
docker start -a hello-world
// prints all the message from hello-world image
```

### Docker start to restart

Docker start can also be used to run previously exited containers.

```bash
docker start -a idOfPreviousContainer
```

`-a flag is the same as --attach`

Note: It does not let you override the executable command again
If we ran `busybox` with `echo hi there`, running the container again with the id will `echo hi there` again as busybox ran with the same command again.

```bash
docker run busybox echo hi there

# hi there
docker ps --all # to get id of the container that ran from a list of
history of containers
# get the id - 123be9848f26
docker start -a 123be9848f26
# hi there - Ran the previous container busybox with the same command again.
```

### Cleaning up containers with `prune`

Docker containers that exited (in stopped start) are still taking up space.They can still be started again with `docker start` command. It might be better to delete all of them to save space once in a while. We use prune to delete the stopped containers.

```bash
docker system prune
```

Docker system gives us some commands to manage docker. One of them is prune.
It deletes,

1. All stopped containers
2. All networks not used by at least one container
3. all dangling images
4. all build cache

### Getting logs from a container using docker logs

We can get the stdout/stderr outputs from the container that ran using docker logs.

```bash
docker logs $containerId
```

[More on logs](https://docs.docker.com/engine/reference/commandline/logs/)

This is useful if we forgot to attach stdout/stderr using the --attach/-a flag for `docker start` command.
This is also a great tool to debug as we are checking stderr as well.
`This does not rerun the container` but only logs the messages it emmited to standard output/error.

It logs and exits. Use --follow flag to keep logging the container's output.

```bash
docker logs --follow
```

### Stopping a docker container with stop

There are two ways to stop a process. By sending either a SIGTERM signal or a SIGKILL signal to the process. SIGTERM gives the process some time before shutting down, to maybe do some clean up. SIGKILL force kills that process.
In docker, we can send a SIGTERM signal to a running container to stop it, giving it some time to shut down as well using `docker stop`

```bash
docker stop $containerId
```

If the container has not shut down within 10 seconds, Docker sends a SIGKILL command, forcefully killing it.
We can also manually send a SIGKILL command using `docker kill`

```bash
docker kill $containerId
```
## Killing all containers
```bash
docker kill $(docker ps -q)
```


Background on Process and std i/o
a process is any active (running) instance of a program.
But what is a program? Well, technically, a program is any executable file held in storage on your machine. Anytime you run a program, you have created a process.
Like a container is an instance of an Image. A container can run processes since it is OS level virtualisation.

Every process has three streams attached

[Wikipedia's definition for streams](https://en.wikipedia.org/wiki/Standard_streams):
In computer programming, standard streams are interconnected input and output communication channels[1] between a computer program and its environment when it begins execution. The three input/output (I/O) connections are called standard input (stdin), standard output (stdout) and standard error (stderr).

So we can use to talk to a process using these three streams.

1. Stdin (Standard input) lets us input information to a process.
2. Stdout (standard output) outputs information from a process.
3. Stderr (standard error) outouts error information from a process.

### The -it flag (-i flag and -t flag)

We can attach to the stdi/o of a process using the -i/--interactive flag.
and we can attach a virtual terminal session to the process using -t flag.

Using only the -i flag lets us access to std i/o but it won't be prettily formatted among other things such as auto-completion done by a terminal.
Using only the -t flag gives a virtual terminal but not attached to any process.

## Executing additional commands in a container

Sometimes we want to execute commands inside one or another container. For example, we might have a container running redix. We can execute command to it with the -it flag( interactively in a pseudo terminal session) with the exec command

```bash
docker exec -it $containerId command
```

Sometimes if we don't give the -it flad, the command might run and kick us out since we cant enter any information anyways.

## Opening up a shell/terminal in a container

We can use `sh` command to open up shell/terminal in containers
We can combine that with `exec` to open shells in any container.
Some containers even has bash built in.

```bash
docker exec -it $containerId sh
```

Note:
`Docker run` opens up the shell with the -it flag. This prevents from the default command/executable from running. This is only if there is the `sh` command configured in the Image. for the hello-world example, there is no shell in that image.

# Section 3: Creating Custom images

## Steps to create a custom image

1. Create Dockerfile

```bash
touch Dockerfile
```

2. Write instructions

```Dockerfile

# Use an existing docker image as a base
FROM alpine

# Download and install a dependency
RUN apk add --update redis

# Tell the image what to do when it starts as a container
CMD [ "redis-server" ]
```

3.Build it with docker client and daemon

```bash
# Inside the folder where Dockerfile is
docker build .
```

# Dockerfile

Dockerfile is what docker daemon uses to build the image.
It is a list of instructions to prepare our image.
Instructions are commands followed by arguments

```Dockerfile
#  COMMAND = FROM, argument = alpinee
FROM alpine
```

Analogy from Stephen Grider
`Writing a Dockerfile == Being given a computer with no OS and being told to install Chrome`

alpine
Alpine is a linux distribution.Imagine we want to install chrome in a computer with no os.
We will first install an operating system
We will use it's default browser to get the chrome installer
We will use file explorer to navigate to the installer
We will execute that installer

So in order to install chrome, we wanted tools an OS provides. This is what alpine does. Among other programs, Alpine has a package manager that helps us install packages such as redis.

# Docker Build .

Command to build a custom image. It takes in a build context. Here ( . ) It passes the build context with the Dockerfile to the docker cli/client which passses to

Docker daemon runs each instruction in the Dockerfile in a temporay container created from the image from the previous instruction

## File flag
Use the -f flag to give a different Dockerfile such as Dockerfile.dev than the default Dockerfile.

1.

```Dockerfile
FROM alpine
# Returns the alpine image
```

Since there is no previous image, docker gets the image from image cache or docker hub and returns an image, lets call it image A

2.

```Dockerfile
RUN apk add --update redis
```

This is an instruction for aline to install redis
Docker creates a new container from image created before (image A which has alpine) and starts running the instruction. It returns a new image with redis inside (Let's call it image B) and destroys the container it was running in.

3

```Dockerfile
CMD ["redis-server]
```

This is an instruction to set the primary executable of an image to the argument provided. We want the image B with redis to always execute `redis-server` when it is run by default.

Docker creates a new container from image B which contains redis and starts running the instruction. It returns a new image (image C ) which has the primary executable command set and destory the intermediary container this was running in.

1.  No Image = >runs FROM alpine => return Image with alpine from hub
2.  Image with alpine => creates an intermediary container from the Image => runs `RUN apk...` => removes intermediary container just created => returns a new image with alpine and redis
3.  Image with alpine and redis => creates an intermiedary container => runs `CMD ['....']` => removes the intermiedary container => returns a new image with alpine and redis with default executable set.

### Building from cache ( docker memoizing with image generation)

`Building docker again images from uses from cache as much as possible`

# Tagging an image

We can tag an image with -t flag. Technically only the version number(right of the :) is the tag. The convention is to use a number or `latest` for the latest one. You still have to provide the build context.

```Dockerfile
docker build -t yourId/nameOfImage:latest .
```

# Commit: Creating an image from a container

We can create an image from a running container (take a snapshot of it's fs and create an executable) by the commit command.

```Dockerfile
docker commit containerId
```

To add a default executable command , use the --change, or -c flag

```Dockerfile
docker commit -c 'CMD ["default command"]' containerId
```

# Copy command in Dockerfile

We can copy files from one directory to a directory in our image using the copy command

```Dockerfile
COPY ./ ./
# copy everything from our project folder(where Dockerfile exists to inside image)
```

# Publish flag: Port mapping

We can publish a container's port to the host's port with --publish or -p flags.

```bash
docker run -p hostPort:contanerPort containerId/name
```

```bash
docker run -p 3000:8080 containerid/name
# another example
docker run -p 3000:3000 containerId/name
```

# Setting Working directory using WORKDIR

WORKDIR helps us set our directory context relative to the argument of WORKDIR
./ becomes whatever WORKDIR is

```Dockerfile
WORKDIR directoryPath
```

common place to put our work is /usr/app

```Dockerfile
WORKDIR /usr/app
```
# Docker Compose
We can automate many of the commands we do with docker cli using Docker compose. Docker compose takes in a yml file named `docker-compose.yml`.


```yml
version: "3"
services:
  redis-server:
    image: "redis"
  node-server:
    build: .
    ports:
      - "5500:3000"
```

Services in docker-compose are very much like containers.
Here first, we specified the version of docker-compose we are going to use.
We wrote the services we want next. We can give them any names.
Service 1 : redis-server
    build and run a container from the image redis
Service 2 :
    build from the current directory,
    this runs only when we have a Dockerfile in the current directory
    we then access the ports in the container. We give it an array and an item with "5500:3000" to map 5500 to 3000 of the container running node-server.


One of the features of creating containers with docker-compose is that it gives us for free networking between containers.

One other feature is automating many of the commands we do with docker cli.

```md
  `docker run my image`

```

## docker-compose down
Docker-compose down stops all containers created using "up".

## docker-system ps
It has to run in the directory where we are running the docker-compose containers.

| Purpose | Cli  | Compose  |
|---|---|--|
|Run a container | docker run image  | docker-compose up  |
| Build and run a container| docker build . and docker run  |  docker-compose up --build |
| Run a container in the background | docker run -d | docker-compose up -d |
|Stop a running container | docker kill/stop | docker-compose down
| status of containers |docker ps| docker-compose psHow to handle docker crashes

Using restart policies
- no
- always
- on-failure: if node process exits with an error code. (code other than 0)
- unless-stopped: always run unless we stop it

We give policty using in yml restart: "value" pair for the service.

if it is no, it has to be in quotes, as `no` in yaml get's interpreted as boolean false. Here we want the string 'no'

```yml
version: "3"
services:
  redis-server:
    image: "redis"
    restart: "no"
  node-server:
    build: .
    restart: on-failure
    ports:
      - "5500:3000"
```


One use case of on-failure is for worker process where we if want to it to restart only if it exits with an error code. (citation needed on worker process)

# Docker volume
Docker volume helps us map the container's files or folders to our local computer's files or folders. This helps us persist data in our local computer running docker container or have development server's like webpack use our local computer's app directory than the container's fs.

we use the -v flag
```bash
docker run -v docker run -v ourFileOrFolder:containerFileorFolder
```
Example with react
```bash
 docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app container/image

```
When we use the colon  (:)  with the -v flag, we are essentially mapping our fs to the container's fs.
When we don't use the colon, we are essentially saying, don't map this file or folder. In this example we are mapping our pwd( existing app folder where react lives) to the app folder in container. But /app/node_modules should be an exception. So the react's webpack dev server in the container will use the node_modules inside for dependencies but listen to changes in our local computer's app directory giving us hot reload while running the app in a container using the dependencies inside the container.

# Production grade workflow with docker

Dockerfile.dev
```Dockerfile
FROM node:alpine

USER node

RUN mkdir -p /home/node/app
WORKDIR /home/node/app

COPY --chown=node:node ./package.json ./

RUN npm install

COPY --chown=node:node ./ ./

CMD ["npm","start"]

```

## docker files
- Production: `Dockerfile`
- development: `Dockerfile.dev`
## Docker commands

| Instructon   | instruction  |
|---|---|
|docker dev build   | docker build -f Dockerfile.dev  |
|docker image tagging| docker build -t devsamrood/imageName:latest .|
|docker port mapping| docker run -p ourPort:containerPort|
|docker volume mapping| docker run -v ourFileOrFolder:containerFileorFolder|

# Docker images to docker hub

Pushing to docker hub docker images is easy.

```bash
docker push imageName

```

image name by convention  is dockerId/nameForImage

# Tests and Node servers

# React hot reloading and jest tests

Create a docker compose file

```bash
version: "3"
services:
  react:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - /home/node/app/node_modules
      - .:/home/node/app
  tests:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /home/node/app/node_modules
      - .:/home/node/app
    command: ["npm","run","test"]
```


# React production workflow

1: Create build folder
2: Serve build folder


```bash
FROM node:alpine as builder

USER node

RUN mkdir -p /home/node/app

WORKDIR /home/node/app

COPY --chown=node:node ./package.json ./

RUN npm install

COPY --chown=node:node ./ ./

CMD ["npm","run","build"]

FROM nginx

COPY --from=builder /home/node/app/build /usr/share/nginx/html

```
