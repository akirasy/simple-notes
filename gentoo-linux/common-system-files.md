---
title  : Common System File
layout : default
parent : Gentoo Linux
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## fstab

```
# /etc/fstab: static file system information.
#
# See the manpage fstab(5) for more information.
#
# NOTE: The root filesystem should have a pass number of either 0 or 1.
#       All other filesystems should have a pass number of 0 or greater than 1.
#
# NOTE: Even though we list ext4 as the type here, it will work with ext2/ext3
#       filesystems.  This just tells the kernel to use the ext4 driver.
#
# NOTE: You can use full paths to devices like /dev/sda3, but it is often
#       more reliable to use filesystem labels or UUIDs. See your filesystem
#       documentation for details on setting a label. To obtain the UUID, use
#       the blkid(8) command.

# <fs>                                          <mountpoint>    <type>  <opts>                  <dump> <pass>

UUID=5FE7-B899                                  /efi            vfat    defaults,noatime        0       0
UUID=4369e0a5-9729-4bca-b0d9-6e4b22529dce       none            swap    sw                      0       0
UUID=7a0c21fc-58e0-4edd-9bef-ba5b72955348       /               btrfs   subvol=@gentoo,noatime  0       0
UUID=8326dc5a-186c-4f27-9456-727955e6ca1f       /home           ext4    defaults,noatime        0       0
```

## make.conf

```
# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
COMMON_FLAGS="-march=x86-64-v3 -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"

# NOTE: This stage was built with the bindist USE flag enabled

# This sets the language of build output to English.
# Please keep this setting intact when reporting bugs.
LC_MESSAGES=C.UTF-8

GRUB_PLATFORMS="efi-64"
MAKEOPTS="-j8 -l8"
EMERGE_DEFAULTS_OPTS="--autounmask=y --autounmask-write"

FEATURES="${FEATURES} getbinpkg binpkg-request-signature"
USE="dist-kernel"
```