# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021-2022 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=dae-geodata
PKG_RELEASE:=1

PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

include $(INCLUDE_DIR)/package.mk

GEOIP_VER:=202604200545
GEOIP_FILE:=geoip.dat.$(GEOIP_VER)
define Download/geoip
  URL:=https://github.com/v2fly/geoip/releases/download/$(GEOIP_VER)/
  URL_FILE:=geoip.dat
  FILE:=$(GEOIP_FILE)
  HASH:=16dbd19ff8dddb69960f313a3b0c0623cae82dc9725687110c28740226d3b285
endef

GEOSITE_VER:=20260424125239
GEOSITE_FILE:=dlc.dat.$(GEOSITE_VER)
define Download/geosite
  URL:=https://github.com/v2fly/domain-list-community/releases/download/$(GEOSITE_VER)/
  URL_FILE:=dlc.dat
  FILE:=$(GEOSITE_FILE)
  HASH:=813c716dc7fd1924fa5dc52751db4916a05ce26757da860d40c026f7cadf4efd
endef

define Package/dae-geodata/template
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=IP Addresses and Names
  URL:=https://www.v2fly.org
  PKGARCH:=all
endef

define Package/dae-geoip
  $(call Package/dae-geodata/template)
  TITLE:=GeoIP List for dae
  PROVIDES:=@dae-geodata
  VERSION:=$(GEOIP_VER)-r$(PKG_RELEASE)
  LICENSE:=CC-BY-SA-4.0
endef

define Package/dae-geosite
  $(call Package/dae-geodata/template)
  TITLE:=Geosite List for dae
  PROVIDES:=@dae-geodata
  VERSION:=$(GEOSITE_VER)-r$(PKG_RELEASE)
  LICENSE:=MIT
endef

define Build/Prepare
	$(call Build/Prepare/Default)
ifneq ($(CONFIG_PACKAGE_dae-geoip),)
	$(call Download,geoip)
endif
ifneq ($(CONFIG_PACKAGE_dae-geosite),)
	$(call Download,geosite)
endif
endef

define Build/Compile
endef

define Package/dae-geoip/install
	$(INSTALL_DIR) $(1)/usr/share/dae
	$(INSTALL_DATA) $(DL_DIR)/$(GEOIP_FILE) $(1)/usr/share/dae/geoip.dat
endef

define Package/dae-geosite/install
	$(INSTALL_DIR) $(1)/usr/share/dae
	$(INSTALL_DATA) $(DL_DIR)/$(GEOSITE_FILE) $(1)/usr/share/dae/geosite.dat
endef

$(eval $(call BuildPackage,dae-geoip))
$(eval $(call BuildPackage,dae-geosite))
