---
title  : steam
layout : default
parent : Games
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Installation

Steam is a video game digital distribution service by Valve. The easiest installation is via `flatpak`.

1. **Install `Steam` via flatpak.**

    ```
    flatpak install flathub com.valvesoftware.Steam
    ```

2. **Install necessary `udev` rules for game controllers.**

    ```
    emerge --ask games-util/game-device-udev-rules
    ```

## Shared Multi-Desktop-User Steam Library

This guide documents how to configure a shared Steam library directory on a Btrfs subvolume for 
multiple users on the same Linux system using group ownership, the SGID bit, and POSIX 
Access Control Lists (ACLs).

### A. Configuration Steps

1. **Create a Shared User Group**

    Create a dedicated system group for all users who will share the Steam library:
    
    ```
    sudo groupadd steam-users
    ```

2. **Add Users to the Shared Group**

    Add existing user accounts to the steam-users group (replace user1, user2, user3 with actual system usernames):
    
    ```
    sudo usermod -aG steam-users user1
    sudo usermod -aG steam-users user2
    sudo usermod -aG steam-users user3
    ```

    > **Note:** Added users must log out and back in for new group memberships to take effect.

3. **Set Directory Ownership**

    Assign user ownership to root and group ownership to steam-users:
    
    ```
    sudo chown root:steam-users /mnt/steamlib
    ```

4. **Configure Group Permissions and SGID**

    Grant full read, write, and execute permissions to group members (2770). The leading 2 sets the SGID bit on the directory, ensuring all newly created files and subdirectories automatically inherit the parent directory's group ownership (steam-users):
    
    ```
    sudo chmod 2770 /mnt/steamlib
    ```

5. **Enforce Permission Inheritance with POSIX ACLs**

    Set default Access Control Lists (ACLs) so that future files and subdirectories created inside the shared folder automatically receive read, write, and execute permissions for members of steam-users:
    
    ```
    sudo setfacl -m g:steam-users:rwX -d -m g:steam-users:rwX /mnt/steamlib
    ```

### B. Set Steam Client to use the library

For each user on the system:

1. Open `Steam`.
2. Go to `Settings` > `Storage`.
3. Click the storage location dropdown at the top and select `Add Drive`.
4. Navigate to `/mnt/steamlib` and click `Add` to set it as an active storage location.
