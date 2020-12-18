---
layout: default
title : Termux
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

## Install Pillow
{:toc}
Using `pip install Pillow` doesn't work. Try this instead:
```
apt install libjpeg-turbo
```
```
LDFLAGS="-L/system/lib/" \
CFLAGS="-I/data/data/com.termux/files/usr/include/" \
pip install Pillow
```
> Source: [Termux Wiki](https://wiki.termux.com/wiki/Image_Editors#Python)

## Get deb files
Install `gnupg` package
```
apt install gnupg
```
Add apt sources using command: `apt edit-sources`.
```
deb [arch=i386] http://deb.debian.org/debian stable main
```
Run `apt update`.<br>
You will encounter missing-key error.<br>
Copy the missing-key and then add the missing key using this command.
```
apt-key adv --keyserver hkp://pool.sks-keyservers.net:80 --recv-keys {THE_MISSING_KEY_HERE}
```
Now you can download any package using:
```
apt download {package}
```
