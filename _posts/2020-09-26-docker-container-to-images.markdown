---
layout: post
title:  "Docker Container to image + Docker Hub"
date:   2020-09-26 12:13:11 +0800
categories: Docker
comments: true
---

# Create Remote Repository in hub.docker.com
- Log in on https://hub.docker.com/
- Click on Create Repository.
- Choose a name (e.g. docker-sql1) and a description for your repository and click Create.


# Log into the Docker Hub from the command line
```bash
docker login --username=yourhubusername --email=youremail@company.com
#WARNING: login credentials saved in /home/username/.docker/config.json
#Login Succeeded
```

# To Commit a container a running container

```bash
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker commit c3f279d17e0a  leokwok2001/docker-sql1
```

# Check the image ID using
```bash
docker images
```

# and tag your image
```bash
docker tag bb38976d03cf yourhubusername/verse_gapminder:firsttry
```