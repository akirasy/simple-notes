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

## Enabling Audio (PipeWire/Pulse) in Podman/Docker

To get sound working in a container (especially on Raspberry Pi / Debian Bookworm), you must bridge
the application (ALSA) to the host's sound server (PipeWire/Pulse) using a Unix socket.

### 1. The Containerfile Logic
You need three things: the client library, the ALSA plugin, and the configuration.

#### For Debian (Bookworm/Slim)

```dockerfile
# Install dependencies
RUN apt-get update && apt-get install -y \
    libpulse0 \
    alsa-utils \
    libasound2-plugins \
    && rm -rf /var/lib/apt/lists/*

# Redirect ALSA to PulseAudio/PipeWire
RUN echo 'pcm.!default { type pulse }' > /etc/asound.conf && \
    echo 'ctl.!default { type pulse }' >> /etc/asound.conf
```

#### For Alpine Linux

```dockerfile
# Install dependencies (Note the specific plugin package)
RUN apk add --no-cache \
    alsa-utils \
    libpulse \
    alsa-plugins-pulse

# Redirect ALSA to PulseAudio/PipeWire
RUN echo 'pcm.!default { type pulse }' > /etc/asound.conf && \
    echo 'ctl.!default { type pulse }' >> /etc/asound.conf
```

### 2. The Execution Command

When running the container, you must map the host's socket and match the User ID (UID).
```bash
podman run -it --rm \
    --device /dev/snd \
    -v /run/user/1000/pulse/native:/run/user/1000/pulse/native \
    -e PULSE_SERVER=unix:/run/user/1000/pulse/native \
    --group-add keep-groups \
    <image_name>
```

Key Flags Explained:
* --device /dev/snd: Access to sound hardware nodes.
* -v ...: Maps the PipeWire communication socket.
* -e PULSE_SERVER=...: Tells the client libraries where to send the audio data.
* --group-add keep-groups: (Podman specific) Inherits your user's audio group permissions from the host.

### 3. Troubleshooting

If sound fails, check these in order inside the container:
* Check Socket Connection: `pactl info` (requires `pulseaudio-utils` or `alpine-conf` packages).
* Test Audio: `speaker-test -t wav -c 2`
* Missing Plugin Error: If you see `Cannot open shared library libasound_module_pcm_pulse.so`, the `ALSA-to-Pulse` bridge package is missing.
* Permission Error: Verify the socket on the host (`ls -la /run/user/1000/pulse/native`) matches the `UID` used inside the container.

