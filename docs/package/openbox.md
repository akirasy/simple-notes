---
layout : default
title  : OpenBox
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

## Default configuration
This can be done by copying them from the global `/etc/xdg/openbox`.
```
mkdir -p ~/.config/openbox
cp -a /etc/xdg/openbox/ ~/.config/
```

## Using [xinit](https://packages.debian.org/buster/xinit) to start [openbox](https://packages.debian.org/buster/openbox)
Copy config file and then start openbox using `startx`.
```
cp /etc/X11/xinit/xinitrc ~/.xinitrc
startx
```
