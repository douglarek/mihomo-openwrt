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

#### I can't install the ipk due to dependency issues

This is normal because the mihomo package itself relies on the kernel kmod. Different builds of the kernel kmod may have different versions, and if they differ, installation will fail. However, we can manually install the current system version of kmod and ignore dependencies when installing mihomo.

```
opkg install kmod-tun kmod-inet-diag kmod-netlink-diag # Manual installation
opkg install mihomo*.ipk --nodeps
```

It is normal to encounter the following error during installation, which indicates that the package has been installed successfully.

```
Installing mihomo (1.18.6-r1) to root...
Configuring mihomo.
Collected errors:
 * pkg_hash_check_unresolved: cannot find dependency kernel (= 6.6.39~d8d97f68b06125f9b2a13d44b337b50f-r1) for kmod-tun
 * pkg_hash_check_unresolved: cannot find dependency kernel (= 6.6.39~d8d97f68b06125f9b2a13d44b337b50f-r1) for kmod-inet-diag
 * pkg_hash_check_unresolved: cannot find dependency kernel (= 6.6.39~d8d97f68b06125f9b2a13d44b337b50f-r1) for kmod-netlink-diag
```
