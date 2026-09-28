---
title  : distrobox
layout : default
parent : Linux Packages
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


## Run Distrobox using Podman (instead of Docker)

To force Distrobox to use Podman when creating a container, set the `DBX_CONTAINER_MANAGER` environment variable:

```
DBX_CONTAINER_MANAGER="podman" distrobox create \
  --name debian \
  --image debian:latest \
  --home ~/Container/distrobox/debian
```

> (Alternatively, set `container_manager="podman"` in `~/.config/distrobox/distrobox.conf` to make it default).

## Create an Instance with its Own Home Folder

Pass the `--home` (or `-H`) flag during creation to isolate configs, cache, and dotfiles:

```
distrobox create \
  --name <container_name> \
  --image <image_name> \
  --home ~/Path/To/Custom/Home \
  --container-manager podman
```

## View Container Initialization Logs

If `distrobox enter` hangs during the "Installing basic packages..." step, stream the live installation logs from a separate terminal tab:

```
podman logs -f <container_name>
```

*(e.g., `podman logs -f debian`)*