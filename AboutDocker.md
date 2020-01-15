# Docker

> "Docker is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers. Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels."

> Run software services in containers. (ie. nginx, apache, mysql, mongodb, drupal wordpress). Solves running different development environments on different systems.

## Docker Engine

> Docker Engine hosts and runs containers from the container image file. It also manages networks, and storage volumes. There are two key aspects to securing Docker Engine: namespaces and cgroups.

### Docker Engine Security

#### Namespaces

> Namespaces is a feature Docker inherits from the Linux Kernel. Namespaces isolate containers from each other so that each process within a container has no visibility into a process running in a neighboring container.

> Initially, Docker containers were run as root users by default, which was cause for a lot of concern. However, since v1.10, Docker supports namespaces, and you can run containers as non-root users. Namespaces are switched off by default in Docker, and need to be activated.

#### cgroups

> cgroups in Docker allow you to set limits for CPU, memory, networking, and block IO. By default containers can use an unlimited amount of system resources, so it’s important to set limits so that the entire system is not affected by a single hungry container.

#### SELinux, AppArmor, and SecComp

> Apart from namespaces and cgroups, Docker Engine can be further hardened by the use of additional tools like SELinux and AppArmor.

> SELinux provides access control for the kernel. It can manage access based on the type of process running in the container, or the level of the process. Based on this, it either enables or restricts access to the host.

> AppArmor attaches a security profile to every process running on a host. The profile defines what resources a process can utilize. Docker applies a default profile to processes, but you can apply a custom profile as well.

> Similar to AppArmor, SecComp uses security profiles to restrict the number of calls a process can make. That rounds off the list of Linux-based kernel security features available in Docker Engine.

## Docker Hub

> While Docker Engine manages containers, it needs the other half of the Docker stack to pull container images from. That part is Docker Hub—the container registry where container images are stored and shared.

> Container images can be created by anyone, and made publicly available for anyone to download. This is both a good and a bad thing. It’s good because it enables collaboration between developers, and makes it extremely easy to spin up an instance of an operating system or an app with just a few clicks. However, it could turn bad if you download a public container image that has a vulnerability.

> The rule of thumb is to always download official repositories, which are available for most common tools, and never download repositories from unknown authors. On top of this, each downloaded container image needs to be scanned for vulnerabilities.

> Docker Hub scans downloaded container images. It scans a few repositories for free, after which you need to pay for scanning as an add-on.

> Docker Hub isn’t the only registry service. Other popular registries include Quay, AWS ECR, and GitLab Container Registry. These tools also have scanning capabilities of their own. Further, Docker Trusted Registry (DTR) can be installed behind your firewall for a fee.

## Docker Security

Read the official Docker documentation on security [here](https://docs.docker.com/engine/security/security/).

There are four major areas to consider when reviewing Docker security:

- the intrinsic security of the kernel and its support for namespaces and cgroups;
- the attack surface of the Docker daemon itself;
- loopholes in the container configuration profile, either by default, or when customized by users.
- the “hardening” security features of the kernel and how they interact with containers.

## Docker vs VM

Docker = server, os + container and app for each instance
VM = Server + vm, os, and app for each instance

Docker has less over-head and is an easier solution to distribute development environments.

## Installation

Download Docker from [Docker](www.docker.com)

After installation:

```sh
docker version
```

### New Container

Container = management command
Run = sub-command
-it = interactive (run in foreground)
--publish (or -p)
80:80 = local port : port from the container
nginx = image name

```sh
docker container run -it --publish 80:80 nginx
```

or Detached (background)
-d = detached
--name = add custom name

```sh
docker container run -d -p 8080:80 --name mynginx nginx
```

:pushpin: `container` is optional but good practice.

:pushpin: view detached containers with `docker container ls` or `docker ps`

:pushpin: stop a container with `docker container stop name`

:pushpin: local ports can be freely assigned but it's good practice to match default listed on hub.docker.com documentation per application (ie. mongodb, apache, nginx...)

:pushpin: if no local image exists it pulls the latest image from [Docker Hub](hub.docker.com).

## Docker Hub

> [Docker Hub](hub.docker.com) is a service provided by Docker for finding and sharing container images.

Similar to GitHub or NPM, search the image of interest here first, find the official release, and use that as the image name when creating a container or pulling an image. Docker Hub also contains important documentation for images, including ports, environment variables and more.

## Commands

### View Information

```sh
docker info
```

### View Running Containers

```sh
docker container ls
```

or

```sh
docker ps
```

### View All Containers

```sh
docker container ls -a
```

### Stop Running Container

```sh
docker container stop name
```

### Remove Container

id = first few characters of container id

```sh
docker container rm id
```

### Remove Running Container

```sh
docker container rm name -f
```

### Remove Multiple Containers

```sh
docker container rm id id id
```

### Remove All Containers

```sh
docker rm $(docker ps -aq)
```

### View Images on Machine

```sh
docker images
```

### Remove Image

```sh
docker  image rm id
```

### Pull Image

```sh
docker pull name
```

## Environment Variables

Environment variables can be found on [Docker Hub](hub.docker.com).

An example of environment variables with MySQL:

```sh
docker container run -d -p 3306:3306 --name mysql --env MYSQL_ROOT_PASSWORD=1234 mysql
```

## Volume / Bind Mount / Bash

### Bash

```sh
docker container exec -it name bash
```

returns:

```sh
root@id:/
```

To view files:

```sh
root@id:/ ls
```

To exit:

```sh
root@id:/ exit
```

### Volume / Bind Mount

Binds local directory to directory in container
\$(pwd) = current local directory

Using nginx as an example:

```sh
docker container run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html --name nginx-website nginx
```

### Docker File / Create Image

> Dockerfile

```
FROM nginx:latest

WORKDIR /usr/share/nginx/html

COPY . .
```

:pushpin: `COPY . .` means copy all to all.

```sh
docker image build -t username/name .
```

Check with:

```sh
docker images
```

Push to Docker account:

```sh
docker push username/name
```

:pushpin: if prompted for authentication use `docker login`
