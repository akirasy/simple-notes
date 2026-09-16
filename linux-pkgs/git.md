---
title  : git
layout : default
parent : Linux Packages
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

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