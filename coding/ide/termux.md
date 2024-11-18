---
title  : Termux
layout : default
parent : IDE
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Settings

Open up `~/.termux/termux.properties` and browse for yourself.
The config file has good _comments_ and is self-explanatory.

## Extra-key configuration

This is my setting.
```
extra-keys = [[ESC, TAB, '=', ':', '~', '-', '_']]
```

## Install Pillow

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
