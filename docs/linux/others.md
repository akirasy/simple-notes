---
layout : default
title  : Others
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

## Change default terminal emulator
Use `update-alternatives`.
```
update-alternatives --config x-terminal-emulator
```

## Using swap file (instead of swap partition)
Create a swapfile
```
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
```
> `/swapfile` is the path and name of the swap file. You can change this to something else.<br>
The number for `count=1048576` equals 1GB. To increase size, multiply that number by 4 if 4GB swap is desired.

Set the swap file permission to 600
```
sudo chmod 600 /swapfile
```
Format the newly created file as swap
```
sudo mkswap /swapfile
```
Enable the newly created swap file
```
sudo swapon /swapfile
```
> To verify if the new swap file is in use, run `swapon -s`

Add entry to `/etc/fstab`
```
/swapfile none swap sw 0 0
```
