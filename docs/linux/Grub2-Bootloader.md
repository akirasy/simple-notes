---
layout: default
title : Grub Bootloader
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

## Word of advise
- Always, always create backup of config file.
- Make sure you have bootable usb drive to fix if anything goes wrong.

To create backup of original setting file, run:
```
cp /etc/default/grub /etc/default/grub.orig
```

## Hide or edit timer on GRUB bootloader
Edit `/etc/default/grub` to these setting
```
GRUB_TIMEOUT=0
```
> Change the number (in seconds) to shorten or prolong grub screen.

Then update grub bootloader
```
sudo update-grub
```

## Boot to last-used OS
Edit `/etc/default/grub` to these setting
```
GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT=true
```
Then update grub bootloader
```
sudo update-grub
```

## Rename OS menuentry
Either way is fine, it depends on one's preference actually.<br>
However, do note that changing these setting will prevent `grub` to update `kernel`.<br>
If you want to update `kernel`, just give the file its execution right and run `sudo update-grub`.

### Easy way
Edit file `/boot/grub/grub.cfg`
```
menuentry "Something" {
    set root=(hdX,Y)
    -- boot parameters --
}
```
Change that `Something` to your desired name.<br>
However, do not run `sudo update-grub`. Otherwise `grub.cfg` will be regenerated to `grub-script` setting.

### A bit complicated way
Open file `/boot/grub/grub.cfg` and look for these headers.
```
### BEGIN /etc/grub.d/10_linux ###
    ***
    ***

### BEGIN /etc/grub.d/30_os-prober ###
    ***
    ***
```
Copy your desired menuentry and paste-append it to `/etc/grub.d/40_custom`.
> The `grub man` says to be sure to not leave empty line at the end of file. I haven't tried it, so I don't know what happen if we ignore the warning.

Then revoke executable right for these files.
```
/etc/grub.d/10_linux
/etc/grub.d/30_os-prober
```
And lastly, update grub to make changes.
```
sudo update-grub
```
