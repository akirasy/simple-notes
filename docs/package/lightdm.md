---
layout : default
title  : LightDM
parent: Package
---

# {{ page.title }}
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Enable autologin in [LightDM](https://packages.debian.org/buster/lightdm)
Edit `/etc/lightdm/lightdm.conf` to these config.
```
autologin-guest=false
autologin-user={username}
autologin-user-timeout=0
```
