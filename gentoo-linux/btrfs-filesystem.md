---
title  : Btrfs Filesystem
layout : default
parent : Gentoo Linux
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Introduction

Btrfs (B-tree File System) is a modern copy-on-write (COW) filesystem for Linux, designed for fault tolerance, self-healing, and easy administration. It supports advanced features like snapshots, subvolumes, transparent compression, and integrated multi-device RAID.

## Basic Commands

- Mount top-level btrfs partition
    
    ```
    mount /dev/sdX /mnt/gentoo
    ```

- Create a Btrfs filesystem:
    
    ```
    mkfs.btrfs -L mylabel /dev/sdX
    ```

- Create subvolume:
    
    ```
    btrfs subvolume create @home
    ```

- Create a snapshot:
    
    ```
    btrfs subvolume snapshot -r /mnt/source /mnt/snapshots/snap_ro
    ```

- Delete subvolume
    
    ```
    btrfs subvolume delete /path/to/subvolume
    ```

    > Nested-subvolumes needs to be deleted first. Run `btrfs subvolume list` first and delete them individually.

- List subvolume

    ```
    btrfs subvolume list /mount-path
    ```