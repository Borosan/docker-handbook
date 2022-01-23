# Docker cheat sheet

### Check Version

*  [`docker version`](https://docs.docker.com/engine/reference/commandline/version/) shows which version of docker you have running.

### Containers

#### Lifecycle

* [`docker create`](https://docs.docker.com/engine/reference/commandline/create) creates a container but does not start it.
* [`docker rename`](https://docs.docker.com/engine/reference/commandline/rename/) allows the container to be renamed.
* [`docker run`](https://docs.docker.com/engine/reference/commandline/run) creates and starts a container in one operation.
* [`docker rm`](https://docs.docker.com/engine/reference/commandline/rm) deletes a container.
* [`docker update`](https://docs.docker.com/engine/reference/commandline/update/) updates a container's resource limits.

#### Starting and Stopping

* [`docker start`](https://docs.docker.com/engine/reference/commandline/start) starts a container so it is running.
* [`docker stop`](https://docs.docker.com/engine/reference/commandline/stop) stops a running container.
* [`docker restart`](https://docs.docker.com/engine/reference/commandline/restart) stops and starts a container.
* [`docker pause`](https://docs.docker.com/engine/reference/commandline/pause/) pauses a running container, "freezing" it in place.
* [`docker unpause`](https://docs.docker.com/engine/reference/commandline/unpause/) will unpause a running container.
* [`docker wait`](https://docs.docker.com/engine/reference/commandline/wait) blocks until running container stops.
* [`docker kill`](https://docs.docker.com/engine/reference/commandline/kill) sends a SIGKILL to a running container.
* [`docker attach`](https://docs.docker.com/engine/reference/commandline/attach) will connect to a running container.

#### 

**CPU Constraints**

You can limit CPU, either using a percentage of all CPUs, or by using specific cores.

For example, you can tell the [`cpu-shares`](https://docs.docker.com/engine/reference/run/#/cpu-share-constraint) setting. The setting is a bit strange -- 1024 means 100% of the CPU, so if you want the container to take 50% of all CPU cores, you should specify 512:

```text
docker run -it -c 512 agileek/cpuset-test
```

You can also only use some CPU cores using [`cpuset-cpus`](https://docs.docker.com/engine/reference/run/#/cpuset-constraint)`:`

```text
docker run -it --cpuset-cpus=0,4,6 agileek/cpuset-test
```

> Note that Docker can still **see** all of the CPUs inside the container -- it just isn't using all of them.

**Memory Constraints**

You can also set [memory constraints](https://docs.docker.com/engine/reference/run/#/user-memory-constraints) on Docker:

```text
docker run -it -m 300M ubuntu:14.04 /bin/bash
```

#### Info

* [`docker ps`](https://docs.docker.com/engine/reference/commandline/ps) shows running containers.
* [`docker logs`](https://docs.docker.com/engine/reference/commandline/logs) gets logs from container.
* [`docker inspect`](https://docs.docker.com/engine/reference/commandline/inspect) looks at all the info on a container \(including IP address\).
* [`docker events`](https://docs.docker.com/engine/reference/commandline/events) gets events from container.
* [`docker port`](https://docs.docker.com/engine/reference/commandline/port) shows public facing port of container.
* [`docker top`](https://docs.docker.com/engine/reference/commandline/top) shows running processes in container.
* [`docker stats`](https://docs.docker.com/engine/reference/commandline/stats) shows containers' resource usage statistics.
* [`docker diff`](https://docs.docker.com/engine/reference/commandline/diff) shows changed files in the container's FS.

#### Import / Export

* [`docker cp`](https://docs.docker.com/engine/reference/commandline/cp) copies files or folders between a container and the local filesystem.
* [`docker export`](https://docs.docker.com/engine/reference/commandline/export) turns container filesystem into tarball archive stream to STDOUT.

#### Import/Export container

Import a container as an image from file:

```text
cat my_container.tar.gz | docker import - my_image:my_tag
```

Export an existing container:

```text
docker export my_container | gzip > my_container.tar.gz
```

#### Executing Commands

* [`docker exec`](https://docs.docker.com/engine/reference/commandline/exec) to execute a command in container.

### Images

#### Lifecycle

* [`docker images`](https://docs.docker.com/engine/reference/commandline/images) shows all images.
* [`docker import`](https://docs.docker.com/engine/reference/commandline/import) creates an image from a tarball.
* [`docker build`](https://docs.docker.com/engine/reference/commandline/build) creates image from Dockerfile.
* [`docker commit`](https://docs.docker.com/engine/reference/commandline/commit) creates image from a container, pausing it temporarily if it is running.
* [`docker rmi`](https://docs.docker.com/engine/reference/commandline/rmi) removes an image.
* [`docker load`](https://docs.docker.com/engine/reference/commandline/load) loads an image from a tar archive as STDIN, including images and tags \(as of 0.7\).
* [`docker save`](https://docs.docker.com/engine/reference/commandline/save) saves an image to a tar archive stream to STDOUT with all parent layers, tags & versions \(as of 0.7\).

#### Info

* [`docker history`](https://docs.docker.com/engine/reference/commandline/history) shows history of image.
* [`docker tag`](https://docs.docker.com/engine/reference/commandline/tag) tags an image to a name \(local or registry\).

#### Load/Save image

Load an image from file:

```text
docker load < my_image.tar.gz
```

Save an existing image:

```text
docker save my_image:my_tag | gzip > my_image.tar.gz
```

### Dockerfile

#### Instructions

* [.dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
* [FROM](https://docs.docker.com/engine/reference/builder/#from) Sets the Base Image for subsequent instructions.
* [MAINTAINER \(deprecated - use LABEL instead\)](https://docs.docker.com/engine/reference/builder/#maintainer-deprecated) Set the Author field of the generated images.
* [RUN](https://docs.docker.com/engine/reference/builder/#run) execute any commands in a new layer on top of the current image and commit the results.
* [CMD](https://docs.docker.com/engine/reference/builder/#cmd) provide defaults for an executing container.
* [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose) informs Docker that the container listens on the specified network ports at runtime. NOTE: does not actually make ports accessible.
* [ENV](https://docs.docker.com/engine/reference/builder/#env) sets environment variable.
* [ADD](https://docs.docker.com/engine/reference/builder/#add) copies new files, directories or remote file to container. Invalidates caches. Avoid `ADD` and use `COPY` instead.
* [COPY](https://docs.docker.com/engine/reference/builder/#copy) copies new files or directories to container. By default this copies as root regardless of the USER/WORKDIR settings. Use `--chown=<user>:<group>` to give ownership to another user/group. \(Same for `ADD`.\)
* [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint) configures a container that will run as an executable.
* [VOLUME](https://docs.docker.com/engine/reference/builder/#volume) creates a mount point for externally mounted volumes or other containers.
* [USER](https://docs.docker.com/engine/reference/builder/#user) sets the user name for following RUN / CMD / ENTRYPOINT commands.
* [WORKDIR](https://docs.docker.com/engine/reference/builder/#workdir) sets the working directory.
* [ARG](https://docs.docker.com/engine/reference/builder/#arg) defines a build-time variable.
* [ONBUILD](https://docs.docker.com/engine/reference/builder/#onbuild) adds a trigger instruction when the image is used as the base for another build.
* [STOPSIGNAL](https://docs.docker.com/engine/reference/builder/#stopsignal) sets the system call signal that will be sent to the container to exit.
* [LABEL](https://docs.docker.com/config/labels-custom-metadata/) apply key/value metadata to your images, containers, or daemons.
* [SHELL](https://docs.docker.com/engine/reference/builder/#shell) override default shell is used by docker to run commands.
* [HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck) tells docker how to test a container to check that it is still working.

### Registry & Repository

* [`docker login`](https://docs.docker.com/engine/reference/commandline/login) to login to a registry.
* [`docker logout`](https://docs.docker.com/engine/reference/commandline/logout) to logout from a registry.
* [`docker search`](https://docs.docker.com/engine/reference/commandline/search) searches registry for image.
* [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull) pulls an image from registry to local machine.
* [`docker push`](https://docs.docker.com/engine/reference/commandline/push) pushes an image to the registry from local machine.

### Volumes

#### Lifecycle

* [`docker volume create`](https://docs.docker.com/engine/reference/commandline/volume_create/)
* [`docker volume rm`](https://docs.docker.com/engine/reference/commandline/volume_rm/)

#### Info

* [`docker volume ls`](https://docs.docker.com/engine/reference/commandline/volume_ls/)
* [`docker volume inspect`](https://docs.docker.com/engine/reference/commandline/volume_inspect/)

### Networks

#### Lifecycle

* [`docker network create`](https://docs.docker.com/engine/reference/commandline/network_create/) NAME Create a new network \(default type: bridge\).
* [`docker network rm`](https://docs.docker.com/engine/reference/commandline/network_rm/) NAME Remove one or more networks by name or identifier. No containers can be connected to the network when deleting it.

#### Info

* [`docker network ls`](https://docs.docker.com/engine/reference/commandline/network_ls/) List networks
* [`docker network inspect`](https://docs.docker.com/engine/reference/commandline/network_inspect/) NAME Display detailed information on one or more networks.

#### Connection

* [`docker network connect`](https://docs.docker.com/engine/reference/commandline/network_connect/) NETWORK CONTAINER Connect a container to a network
* [`docker network disconnect`](https://docs.docker.com/engine/reference/commandline/network_disconnect/) NETWORK CONTAINER Disconnect a container from a network

.

.

.

------

[https://github.com/wsargent/docker-cheat-sheet\#dockerfile](https://github.com/wsargent/docker-cheat-sheet#dockerfile)



