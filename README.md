# Vanilla-Mihomo

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

You can download the latest release [here](https://github.com/douglarek/vanilla-mihomo/actions).
