---
layout: post
title: Docker Notes
---

This are my notes about `docker` and how to run it for my projects.
I'm sharing them here for convenience of the teams that I'm working with.
Note: This is a live document, expect changes any time.

# Docker

## Basics

Docker can be seen as a light-weight virtual machine runner.
It's not a true virtual machine, but it is enough to provide virtualized environments for servers and clouds.
In that context, being light-weight is an advantage, as it consumes less memory than a virtual machine, and the integration with the host environment is more powerful than that of a virtual machine.


## OS Support

Docker comes with Linux. Eventually install it using `sudo apt-get intall docker`.
Docker does not run out-of-the-box on Mac or Windows, but it can be run using third-party tools like `boot2docker`.


## Running Docker

Docker is a command with subcommands to run, similar to `git` or `svn`.

To see what docker can do, simply run `docker`.


## Getting, installing and running images

To see what images are available on your system, use `docker images`
In order to see what images are available for download on a specific term, use `docker search `*`term`*.
An image can be downloaded and installed using the `docker pull` command.
For example, to download and install a ubuntu container, use `docker pull ubuntu`.
To run the container, use `docker run ubuntu`.
That generated a new LXC container, created a new file system, mounted a read/write layer, allocated network interface etc. whatever is required for running a container.
You probably want to run it with a specific command in the container, `docker run -it ubuntu bash`.

Definition: An image in execution is called *container*.

To see which docker images *available* on the system, use `docker images`.
To see the docker containers, use `docker ps`.


## Running a command inside a docker container

Find out the docker container using `docker ps`.
Then run the command like this `docker exec container-id command`.
For example, if your container is `postgres:latest` with id `343147e64921` and you want to run `echo hello world` in it, run `docker exec 343147e64921 echo hello world`.


## Persistence

The file system per default is in memory (like ramfs).
In order to persist files, use the `docker commit` command.


## History

To view the (commited) history, use `docker history `*`imagename`*.


## Log / stdout

To view the stdout of a container, use `docker logs`.
