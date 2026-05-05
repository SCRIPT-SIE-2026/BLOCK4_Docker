# Docker Commands Cheatsheet

This sheet gathers the most useful Docker commands for the course.

Notation used:

- `<image>`: name of a Docker image, for example `python:3.11`.
- `<container>`: identifier or name of a container.
- `<volume>`: name of a Docker volume.
- `<network>`: name of a Docker network.

## Table of Contents

- [Docker Images](#docker-images)
- [Build an Image from a Dockerfile](#build-an-image-from-a-dockerfile)
- [Launch a Container](#launch-a-container)
- [List Containers](#list-containers)
- [Interact with a Container](#interact-with-a-container)
- [View Logs and Detailed Information](#view-logs-and-detailed-information)
- [Manage a Container's State](#manage-a-containers-state)
- [Remove Containers](#remove-containers)
- [Volumes](#volumes)
- [Networks](#networks)
- [Cleanup and Disk Space](#cleanup-and-disk-space)

## Docker Images

Download an image from a registry, for example Docker Hub:

```bash
docker pull python:3.11
```

List the images available locally:

```bash
docker images
```

Equivalent command, more consistent with Docker's modern subcommands:

```bash
docker image ls
```

Remove a local image:

```bash
docker image rm <image>
```

Equivalent short form:

```bash
docker rmi <image>
```

Example:

```bash
docker rmi python:3.11
```

An image cannot be removed if it is still used by an existing container.

## Build an Image from a Dockerfile

Build an image from the `Dockerfile` located in the current directory:

```bash
docker build -t <image_name>:<tag> .
```

Example for the course project:

```bash
cd SCRIPT_SIE_2026_05_12_Project
docker build -t sie-python:latest .
```

The `.` tells Docker to use the current directory as the build context.

## Launch a Container

Launch a container in interactive mode with an attached terminal:

```bash
docker run -it <image> <command>
```

Example:

```bash
docker run -it python:3.11 bash
```

Launch a container in the background:

```bash
docker run -d <image>
```

Launch a container in the background with an allocated terminal:

```bash
docker run -td <image> <command>
```

Example:

```bash
docker run -td python:3.11 bash
```

Give the container a name:

```bash
docker run --name <container_name> -it <image> <command>
```

Example:

```bash
docker run --name python-test -it python:3.11 bash
```

Automatically remove the container when it stops:

```bash
docker run --rm -it python:3.11 bash
```

Launch the image built in the course project:

```bash
docker run --rm sie-python:latest
```

Mount the current directory inside the container:

```bash
docker run --rm -it -v "$PWD:/app" -w /app python:3.11 bash
```

Important options:

- `-i`: keeps standard input open.
- `-t`: allocates a terminal.
- `-d`: runs the container in the background.
- `--name`: gives the container a readable name.
- `--rm`: removes the container when it stops.
- `-v`: mounts a volume or local directory.
- `-w`: sets the working directory inside the container.

## List Containers

List running containers:

```bash
docker ps
```

List all containers, including stopped ones:

```bash
docker ps -a
```

Equivalent command:

```bash
docker container ls
```

List all containers with the `container` syntax:

```bash
docker container ls -a
```

## Interact with a Container

Run a command in an already running container:

```bash
docker exec <container> <command>
```

Open a shell in an already running container:

```bash
docker exec -it <container> bash
```

Example:

```bash
docker exec -it python-test bash
```

Attach to the main process of a container:

```bash
docker attach <container>
```

Warning: with `attach`, exiting with `Ctrl+C` may stop the container's main process. To open a shell in a running container, `docker exec -it` is often more appropriate.

## View Logs and Detailed Information

Display a container's logs:

```bash
docker logs <container>
```

Follow logs in real time:

```bash
docker logs -f <container>
```

Display detailed information about a container, image, volume, or network:

```bash
docker inspect <object>
```

Examples:

```bash
docker inspect python-test
docker inspect python:3.11
```

## Manage a Container's State

Stop a container cleanly:

```bash
docker stop <container>
```

Start a stopped container:

```bash
docker start <container>
```

Restart a container:

```bash
docker restart <container>
```

Force a container to stop immediately:

```bash
docker kill <container>
```

In practice, prefer `docker stop`. Use `docker kill` only if the container no longer responds.

## Remove Containers

Remove a stopped container:

```bash
docker rm <container>
```

Equivalent command:

```bash
docker container rm <container>
```

Remove all stopped containers:

```bash
docker rm $(docker ps -a -q)
```

If containers are still running, you must stop them before removing them:

```bash
docker stop <container>
docker rm <container>
```

## Volumes

Volumes preserve data independently of a container's lifecycle.

List Docker volumes:

```bash
docker volume ls
```

Create a volume:

```bash
docker volume create <volume>
```

Display detailed information about a volume:

```bash
docker volume inspect <volume>
```

Use a volume in a container:

```bash
docker run --rm -it -v <volume>:/data python:3.11 bash
```

Remove a volume:

```bash
docker volume rm <volume>
```

A volume cannot be removed if it is still used by a container.

## Networks

Docker creates networks to allow containers to communicate with each other.

List Docker networks:

```bash
docker network ls
```

Display detailed information about a network:

```bash
docker network inspect <network>
```

Example:

```bash
docker network inspect bridge
```

In this course, it is mainly useful to know how to inspect networks created automatically by Docker or Docker Compose.

## Cleanup and Disk Space

Display the disk space used by Docker:

```bash
docker system df
```

Remove unused Docker objects:

```bash
docker system prune
```

This command removes stopped containers, unused networks, and unused images in particular. Docker asks for confirmation before removing them.

Also remove unused volumes:

```bash
docker system prune --volumes
```

Use with caution: volumes may contain results or data that you want to keep.
