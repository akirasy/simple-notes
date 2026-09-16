---
title  : openssh
layout : default
parent : Linux Packages
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


## Start `ssh-agent`

```
eval "$(ssh-agent -s)"
```

## Passwordless SSH

1. Generate new SSH key

    ```
    ssh-keygen -C "key-name"
    ```

1. Add `SSH key` to the ssh-agent

    ```
    ssh-add {path/to/SSH_key)
    ```

1. Copy `ssh-key` to the remote server

    ```
    ssh-copy-id remote_username@server_ip_address
    ```

    > SSH key could also be transferred manually. Copy the key into `~/.ssh/` onto the client PC.

1. Setup completed. Enjoy! Just connect to the remote server

    ```
    ssh remote_username@server_ip_address
    ```

    > This method also works for setting up SSH on Github. Just copy the `key.pub` contents into Github settings. 
    
    > Test your SSH connection with Github using `ssh -T git@github.com` afterwards.

## SSH activation script

```
#!/usr/bin/bash

# SSH credentials
SSH_KEY=$HOME/.ssh/github

# Activation script
eval "$(ssh-agent -s)"
ssh-add $SSH_KEY
echo "GitHub credentials activated!"
```

## Start ssh-agent automatically

1. Append `~/.bashrc` file with the following content.

    ```
    if [ -z "$SSH_AUTH_SOCK" ]; then
      eval "$(ssh-agent -s)"
    fi
    ```

1. You might also want to add your `ssh-key` automatically. Just append your key in the `~/.bashrc` file.

    ```
    SSH_KEYS_TO_ADD=(
      "$HOME/.ssh/id_rsa"
      "$HOME/.ssh/id_ed25519"
      # Add more key paths here, one per line
      # "$HOME/.ssh/my_other_key"
    )
    
    for key_path in "${SSH_KEYS_TO_ADD[@]}"; do
      if [ -f "$key_path" ]; then
        echo "Adding key: $key_path"
        ssh-add "$key_path" || echo "Failed to add key: $key_path (check passphrase or file permissions)"
      else
        echo "Warning: Private key file not found: $key_path"
      fi
    done
    ```

