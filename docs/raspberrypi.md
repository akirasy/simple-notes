---
layout : default
title  : Raspberry Pi
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

## Introduction
A very versatile single-board-computer (SBC)

## Enable SSH
For headless setup, SSH can be enabled by placing a file named `ssh`,
without any extension, onto the `boot` partition of the SD card.

## Wi-Fi Network on first boot
Create file `wpa_supplicant.conf` as below:
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<Insert 2 letter ISO 3166-1 country code here>

network={
 ssid="<Name of your wireless LAN>"
 psk="<Password for your wireless LAN>"
}
```
Save the file into `/boot` partition prior to first boot.
