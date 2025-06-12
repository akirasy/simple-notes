---
title  : SSH
layout : default
parent : Coding is Life
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Set up passwordless SSH

1. Start the `ssh-agent` in the background
    ```
    eval "$(ssh-agent -s)"
    ```
1. Generate new SSH key
    ```
    ssh-keygen -C "your_email@example.com"
    ```
1. Add `SSH key` to the ssh-agent
    ```
    ssh-add {path/to/SSH_key)
    ```
1. Copy `ssh-key` to the remote server
    ```
    ssh-copy-id remote_username@server_ip_address
    ```
1. Begin `ssh` to the remote server
    ```
    ssh remote_username@server_ip_address
    ```

## Set up SSH on Github

1. Generate new SSH key
    ```
    ssh-keygen -C "your_email@example.com"
    ```
2. (Optional) Set personalized filename for `SSH_key`
    ```
    > Enter a file in which to save the key (/Users/you/.ssh/id_ed25519): [Press enter]
    ```
3. Start the `ssh-agent` in the background
    ```
    eval "$(ssh-agent -s)"
    ```
4. Add `SSH key` to the ssh-agent
    ```
    ssh-add {path/to/SSH_key)
    ```
5. Copy content of `SSH_key.pub` to your Github account
6. Test your SSH connection with Github
    ```
    ssh -T git@github.com
    ```

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
    # Start ssh-agent if it's not already running

    if [ -z "$SSH_AUTH_SOCK" ]; then
      eval "$(ssh-agent -s)"
    fi
    ```

1. You might also want to add your `ssh-key` automatically. Just append your
key in the `~/.bashrc` file.
    ```
    # Add keys to ssh-agent

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

