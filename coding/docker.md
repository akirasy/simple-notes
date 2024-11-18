---
title  : Docker CLI
layout : default
parent : Coding is Life
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

The Docker command-line interface (Docker CLI) is a robust tool that empowers you to interact with
Docker containers and manage different aspects of the container ecosystem directly from the command line.

## Official links

- [Docker CLI Documentation](https://docs.docker.com/reference/cli/docker/){: .btn .btn-purple }
- [Get Docker](https://docs.docker.com/get-started/get-docker/){: .btn .btn-purple }

## Create & build image

1. Create Dockerfile
    ```
    FROM debian:buster-slim
    
    # Do customization on container
    RUN apt update && apt install -y --no-install-recommends \
    python3-pip python3-setuptools && \
    apt -y autoremove && apt clean

    # Do something exciting here...
    ## exciting..exciting..exciting
    ## It works!
    ```
2. Build
    ```
    docker build -t {container-name} .
    ```

## Run docker in headless-persistence

```
docker run --detach --restart unless-stopped {container}
```

