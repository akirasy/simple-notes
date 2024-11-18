---
title  : Git
layout : default
parent : Coding is Life
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Set up SSH on Github

1. Generate new SSH key
    ```
    ssh-keygen -t ed25519 -C "your_email@example.com"
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

## Activate Git SSH

```
#!/usr/bin/bash

# SSH credentials
SSH_KEY=$HOME/.ssh/github

# Activation script
eval "$(ssh-agent -s)"
ssh-add $SSH_KEY
echo "GitHub credentials activated!"
```

## Using `.gitignore`

1. Single file
    - Put the exact filename.
    ```
    .DS_Store
    ```
2. Directories
    - Include their paths and putting a `/` on the end.
    - If `/` is not present, it will match both files and directories with that name.
    ```
    node_modules/
    logs/
    ```
3. Negation 
    - Use a prefix of `!` to negate a file.
    ```
    !example.log
    ```
4. Double Asterisk    
    - `**` can be used to match any number of directories.<br>
    - `**/logs` matches all files or directories named logs<br>
    - `**/logs/*.log` matches all files ending with `.log` in a logs directory<br>
    - `logs/**/*.log` matches all files ending with `.log` in the logs directory and any of its subdirectories.<br>
    - `logs/**` matches all files inside of logs.
5. Comments    
    - Any lines that start with `#` are comments.
    ```
    # macOS Files
    .DS_Store
    ```

