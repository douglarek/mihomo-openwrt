# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=mihomo
PKG_VERSION:=1.18.6
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/MetaCubeX/mihomo/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=b8f343a02e873d632fb488537f7d1bd2c63af1654075143678570ba848a45df5

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Leo Douglas <douglarek@gmail.com>

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/MetaCubeX/mihomo
GO_PKG_LDFLAGS_X:=$(GO_PKG)/constant.Version=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-values.mk

define Package/mihomo
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=A rule-based tunnel in Go
  URL:=https://github.com/MetaCubeX/mihomo
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle +kmod-tun +kmod-inet-diag +kmod-netlink-diag
endef

define Package/mihomo/description
  Mihomo is a cross-platform rule-based proxy utility that runs on the
  network and application layer, supporting various proxy and
  anti-censorship protocols out-of-the-box.

  It has been adopted widely by the Internet users in some countries
  and regions where the Internet is heavily censored or blocked.
  Either way, Mihomo can be used by anyone who wants to improve their
  Internet experience.
endef

define Package/mihomo/conffiles
/etc/mihomo/config.yaml
/etc/config/mihomo
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	sed -i 's/^VERSION=.*/VERSION ?= $(PKG_VERSION)/' $(PKG_BUILD_DIR)/Makefile
endef

define Package/mihomo/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/mihomo-linux-$(GO_ARCH) $(1)/usr/bin/mihomo
	
	$(INSTALL_DIR) $(1)/etc/mihomo/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/docs/config.yaml $(1)/etc/mihomo/example.yaml
	
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) $(CURDIR)/files/mihomo.config $(1)/etc/config/mihomo
	
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(CURDIR)/files/mihomo.init $(1)/etc/init.d/mihomo
endef

$(eval $(call GoBinPackage,mihomo))
$(eval $(call BuildPackage,mihomo))
