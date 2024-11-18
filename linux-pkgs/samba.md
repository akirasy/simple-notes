---
title  : Samba
layout : default
parent : Linux Packages
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Installation

Use _apt_ to install `samba`.
```
apt install samba
```
> Some guide might say to install `samba-common-bin` too,
but for me installing samba is enough.

## Configuration

Samba provides default config at `/etc/samba/smb.conf`.<br>
I usually backup that default config to `/etc/samba/smb.conf.orig`
and replace it with mine below.
```
#======================= Global Settings =======================
[global]
   server role = standalone server

#======================= Share Definitions =======================
[abah]
comment = File abah
path = /home/pi/samba/abah/
valid users = abah
writeable = yes
create mask = 774
create directory mask = 774

[ibu]
comment = File ibu
path = /home/pi/samba/ibu/
valid users = ibu
writeable = yes
create mask = 774
create directory mask = 774

[media-server]
comment = Movies, music, etc
path = /home/pi/samba/media-server/
valid users = @pi
writeable = yes
create mask = 774
create directory mask = 774
```

## Samba users

Samba uses the server's `user`.
So to implement unique user in samba, follow these steps.
1. Add new user to the server by using:
    ```
    sudo useradd {new_username}
    ```
2. After creating new user, give it a new `samba password` as below:
    ```
    sudo smbpasswd -a {new_username}
    ```
> To modify an existing Samba user’s Samba password use: `smbpasswd {username}`

## User file permissions

Summary of permission using octal numbers.

| Symbolic | Octal  | English              |
| :------: | :----: | :------------------: |
|   - - -  |   0    | No Permission        |
|   - - x  |   1    | Execute              |
|   - - w  |   2    | Write                |
|   - w x  |   3    | Write & Execute      |
|   r - -  |   4    | Read                 |
|   r - x  |   5    | Read & Execute       |
|   - r w  |   6    | Read & Write         |
|   r w x  |   7    | Read, Write, Execute |

In samba config, this can be defined as:
```
create mask = 0744
create directory mask = 0755
```

## Advanced configuration

There are just too many config to be mentioned here.<br>
Go to the [official documentation](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)
for those config together with their default values.<br> 
Use the one that suits your need.
