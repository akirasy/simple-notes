---
layout : default
title  : Advance Packaging Tools
parent: Linux
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

## Enable multiarch repository
Enable 32-bit repository.
```
dpkg --add-architecture i386
```
List other arch.
```
dpkg --print-foreign-architectures
```

## Enable add-apt-repository
Install `software-properties-common`.

## Use ISO file as offline repository
Create folders and mount the ISO files.
```
mkdir -p /repo/source-1/
mount -o loop {file.iso} /repo/source-1/
```
Add entry to `/etc/apt/sources.list`.
```
deb [trusted=yes] file:///repo/source-1/ stable main contrib
```
Run `sudo apt update`.
> Mount does not persist between reboot.<br>
So it's better we create a 'mount script' which runs on every reboot using `crontab`.<br>
I have mine on this [link](https://gist.github.com/akirasy/06f30d1dcae4609e5f48815bc818150c).

## Get deb files for offline host using online host
Run this command on desired to install PC.
```
apt-get install -y {your-package} --print-uris > {filename}
```
Then run this command to delete the unnecessary content.
```
sed -i '1,/After/d' {filename}\
sed -i 's/ .*//' {filename}\
sed -i "s/'/ /g" {filename}
```
The file now contains all links to downlad `*.deb` files.<br>
Just download them with `wget`.
```
wget -c -i {filename}
```
Alternatively, just use this [script](https://gist.github.com/akirasy/7e7d3c28fb573bf35432290e1c741a46).
