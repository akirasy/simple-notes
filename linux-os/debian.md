---
title  : Debian
layout : default
parent : Linux OS
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Introduction

Debian OS is excellent to use for system that requires little maintenance.
All that is needed is to update the security patch regularly.

## Enable multiarch repository

Most modern PC uses 64-bit architecture.
Some packages (ie. Steam) requires 32-bit library.<br>
So we need to enable multiarch feature.<br>

### Enable 32-bit repository.

```
dpkg --add-architecture i386
```

### List other arch.

```
dpkg --print-foreign-architectures
```

## Enable add-apt-repository

Whilst using debian, sometime we need to use _Ubuntu PPA_.
The easiest way to do is by doing this.
Be warned that not every PPA works well Debian.
1. Install `software-properties-common`.
    ```
    apt install software-properties-common
    ```
2. Add repository using `add-apt-repository`.
    ```
    add-apt-repository ppa:<PPA Name>
    ```

## Use ISO file as offline repository

This is useful when installing Debian Lite because `network-manager` is not installed by default.
We need `DVD Debian Installer` to use as the `apt` source.
Follow these steps.
1. Create an empty folder.
    ```
    mkdir -p /home/<username>/apt-source
    ```
2. Mount _ISO file_ to the newly created folder.
    ```
    mount -o loop {file.iso} /home/<username>/apt-source
    ```
3. Add entry to `/etc/apt/sources.list`.
    ```
    deb [trusted=yes] file:///home/<username>/apt-source/ stable main
    ```
4. Update repository on your PC.
    ```
    apt update
    ```
> 1\. `sources.list` file only support absolute filepath.<br>
> 2\. You might need to edit `stable` to the OS name.

## APT Errors

Using offline or ISO repository sometime throws error like this.<br>
A `Check-Valid-Until`  is a yes/no value which controls if APT should try to detect replay attacks.
However, some repositories such as historic archives are not updated any more by design, so this
check can be disabled by setting this option to `false`.
```
apt -o Acquire::Check-Valid-Until=false update
```
