# Docker notes - Stephen Grider

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
