# Mihomo-OpenWrt

A native build of [Mihomo](https://github.com/MetaCubeX/mihomo) designed specifically for OpenWrt and its derivatives without hearse scripts.

## Prerequisites

* OpenWrt/ImmortalWrt 23.05+

## Basic Requirements

To effectively use, you should have:

* Basic knowledge of OpenWrt
* Proficiency in using the OpenWrt terminal
* Familiarity with basic [Mihomo configuration](https://wiki.metacubex.one/en/config/)

## Configuration

Utilizes the `auto-redirect` feature introduced in Mihomo:

```yaml
tun:
  enable: true
  stack: mixed
  dns-hijack:
    - "any:53"
  auto-route: true
  auto-redirect: true # Key configuration
  auto-detect-interface: true
```

Before packaging this project, I explored some usage methods, which can be referenced [here](https://gist.github.com/douglarek/99fb8d7f30fac2a6d2e9a32a47296e30) . It might be helpful.

## Download

You can download the latest release [here](https://github.com/douglarek/vanilla-mihomo/releases). Don't worry about the release time, it will always be the latest.

#### Install

> [!IMPORTANT]
> Starting from November 2024, OpenWrt will use the apk package manager by default. Sorry, this project will only support building APK packages and will no longer support IPK.

```
$ apk add mihomo-1.18.10-r1_aarch64_generic.apk --allow-untrusted
(1/4) Installing kmod-inet-diag (6.6.60-r1)
Executing kmod-inet-diag-6.6.60-r1.post-install
(2/4) Installing kmod-netlink-diag (6.6.60-r1)
Executing kmod-netlink-diag-6.6.60-r1.post-install
(3/4) Installing kmod-tun (6.6.60-r1)
Executing kmod-tun-6.6.60-r1.post-install
(4/4) Installing mihomo (1.18.10-r1)
Executing mihomo-1.18.10-r1.post-install
OK: 222 MiB in 241 packages
```

The APK package manager will automatically install the corresponding kernel module dependencies.
