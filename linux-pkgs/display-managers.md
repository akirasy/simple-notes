---
title  : Display Managers
layout : default
parent : Linux Packages
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Autologin

### SDDM

1. Generate current `sddm.conf` file.
    ```
    sddm --example-config > sddm.conf
    mkdir -p /usr/lib/sddm/sddm.conf.d/
    mv sddm.conf /usr/lib/sddm/sddm.conf.d/
    ```
2. Open the newly generated file. Keep other settings as is and only change these parameters accordingly.
    ```
    [Autologin]
    # Whether sddm should automatically log back into sessions when they exit
    Relogin=true
    
    # Name of session file for autologin session (if empty try last logged in)
    Session=
    
    # Username for autologin session
    User={username}
    ```

### LightDM

Edit `/etc/lightdm/lightdm.conf` to these config.
```
autologin-guest=false
autologin-user={username}
autologin-user-timeout=0
```

