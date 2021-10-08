# Docker notes - Stephen Grider

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
docker exec -it $containerId/containerName command
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
