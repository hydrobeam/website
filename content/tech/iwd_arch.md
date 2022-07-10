---
title: "Arch iwd/WiFi Eduroam Setup - UWaterloo"
date: 2022-07-07
slug: "iwd_setup"
description: "Sample config for Eduroam using `iwd` at the University of Waterloo."
keywords: ["wifi", "arch", "linux", "install", "iwd", "waterloo"]
draft: false
tags: ["linux", "guide"]
math: false
toc: false
---

## Introduction

Setting up [iwd](https://wiki.archlinux.org/title/Iwd) when first installing Arch proved to be quite a
hassle, especially because I was relying on campus WiFi via eduroam. Eduroam has a lot of mandatory configuration options that are abstracted away when using typical network managers.
Since `iwd` is the recommended way of getting connected to the internet during the [arch install](https://wiki.archlinux.org/title/installation_guide), it felt particularly important to document how to set it up for beginners!

> Note: This guide is specific to the University of Waterloo, but the setup is applicable to any other institution which uses `eduroam`

## Config

Here's the config file you should place at `/var/lib/iwd/eduroam.8021x`. Be sure to replace the `EAP-PEAP-Phase2-Identity`/`EAP-PEAP-Phase2-Password` fields!

```toml
[Security]
EAP-Method=PEAP
EAP-Identity=anonymous@uwaterloo.ca
EAP-PEAP-CACert=/etc/ssl/certs/GlobalSign_Root_CA_-_R3.pem
EAP-PEAP-Phase2-Method=MSCHAPV2
EAP-PEAP-Phase2-Identity=WATID@uwaterloo.ca
EAP-PEAP-Phase2-Password=YOUR_PASSWORD
EAP-PEAP-ServerDomainMask=eduroam.uwaterloo.ca

[Settings]
AutoConnect=true
```

You can find the keys to the values in the Arch wiki, but the values are unique to each university, and are obtained 
by digging through the ["eduroam installer"](https://cat.eduroam.org/) (a python script). 

## Connecting

From this, `iwctl` (command line front-end for `iwd`) can get you connected:

```bash
iwctl station wlan0 connect eduroam
```

And you're in!


## Tips

- Make sure everything in the config file is spelled correctly!

- Connecting to non-eduroam networks is much simpler, you just need the password to your network!

- When connecting to other networks, `iwd` will automatically generate config files at `/var/lib/iwd/name_of_network.type_of_network`


### Investigating Errors:

When connecting to a network and it seems to inexplicably not work, you can use:

```bash
systemctl status iwd
```

to better understand what's going wrong. 
