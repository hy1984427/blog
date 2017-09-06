title: Quick Start for Docker
date: 2017-07-18
categories:
- Testing
tags:
- Docker
- Quick-Start
comments: true
---

### Installation
  Download from https://www.docker.com/docker-mac

### Register a Docker account
  Register on https://www.docker.com/

### Check Docker availability
  Run following commands in Terminal
  `docker -v`
  `docker -info`

### Pull a Docker image. e.g. "Ruby"
  `docker pull ruby`

### Check the ruby version in Docker image "Ruby"
  `docker run ruby ruby -v`

### Run Docker image at background and with an alias, e.g. "trail"
  `docker run -idt --name=trial ruby`

#### Check all the running Docker images
  `docker ps`

#### Attach to the running Docker image and execute commands in it
  You can use `docker attach trial`
  But it's better to use `docker exec -i -t trial sh`

**Reference**
[Docker — 从入门到实践](https://yeasy.gitbooks.io/docker_practice/)
