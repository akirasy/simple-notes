---
title  : Linux Swap
layout : default
parent : Linux Configs
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## General idea

Swapping is a technique where data in Random Access Memory (RAM) is written to a
special location on your hard disk—either a swap partition or a swap file—to free up RAM.
Based on Linux documentation:
```
"This control is used to define how aggressive (sic) the kernel will swap memory pages.
Higher values will increase aggressiveness, lower values decrease the amount of swap.
A value of 0 instructs the kernel not to initiate swap until the amount of free and
file-backed pages is less than the high water mark in a zone.

The default value is 60."
```

## Changing swappiness value temporarily

```
sudo sysctl vm.swappiness={value}
```

## Set swappiness value permanently

Open `/etc/sysctl.conf` file and add this line.
```
vm.swappiness={value}
```

## Using swap file (instead of swap partition)

1. Create a swapfile
    ```
    sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
    ```
    > `/swapfile` is the path and name of the swap file. You can change this to something else.<br>
    The number for `count=1048576` equals 1GB. To increase size, multiply that number by 4 if 4GB swap is desired.
2. Set the swap file permission to 600
    ```
    sudo chmod 600 /swapfile
    ```
3. Format the newly created file as swap
    ```
    sudo mkswap /swapfile
    ```
4. Enable the newly created swap file
    ```
    sudo swapon /swapfile
    ```
    > To verify if the new swap file is in use, run `swapon -s`
5. Add entry to `/etc/fstab`
    ```
    /swapfile none swap sw 0 0
    ```
