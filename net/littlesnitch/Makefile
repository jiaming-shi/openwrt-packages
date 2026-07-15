# SPDX-License-Identifier: GPL-2.0-or-later
#
# Little Snitch for Linux - OpenWrt package (multi-arch)
#
# Repackages the upstream static musl binary into an apk package.
# The binary embeds eBPF objects and BTF metadata that must not be stripped.

include $(TOPDIR)/rules.mk

PKG_NAME:=littlesnitch
PKG_VERSION:=1.1.0
PKG_RELEASE:=1

PKG_MAINTAINER:=Jiaming Shi <jiaming.shih@gmail.com>
PKG_LICENSE:=Proprietary
PKG_LICENSE_FILES:=

ifeq ($(ARCH),x86_64)
  UPSTREAM_ARCH:=amd64
  PKG_HASH:=f1d44335fdea7abd3f0bd418197b88a65d9024ab099580e7179951148bd07ce1
else ifeq ($(ARCH),aarch64)
  UPSTREAM_ARCH:=arm64
  PKG_HASH:=9fa387de3ad4d5510665b94ae3156f308294c673f6b1692907ac6eb6294aeea3
else ifeq ($(ARCH),powerpc64)
  UPSTREAM_ARCH:=ppc64le
  PKG_HASH:=6d9a47378059c793f2465c901970647a0186a57f2e4d1725c3e7cf681a4dac08
else ifeq ($(ARCH),riscv64)
  UPSTREAM_ARCH:=riscv64
  PKG_HASH:=46ed4b6e8fc28f513348f2b04bfd4461c762830ecddf9e103e92fc9398849b86
endif

PKG_SOURCE:=littlesnitch-$(PKG_VERSION)-$(UPSTREAM_ARCH)-linux-musl.tar.gz
PKG_SOURCE_URL:=https://obdev.at/downloads/littlesnitch-linux/

include $(INCLUDE_DIR)/package.mk

# Prevent the build system from stripping BTF / eBPF sections out of the binary.
RSTRIP:=:
STRIP:=:

define Package/littlesnitch
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Firewall
  TITLE:=Little Snitch for Linux (outbound eBPF firewall)
  URL:=https://obdev.at/products/littlesnitch-linux/index.html
  DEPENDS:=@(aarch64||x86_64||powerpc64||riscv64) +ca-bundle
endef

define Package/littlesnitch/description
 Little Snitch for Linux is an outbound application firewall based on eBPF.

 This package installs the official statically-linked musl binary from
 Objective Development. It embeds GPL-2.0 components (eBPF programs and
 web UI from https://github.com/obdev/littlesnitch-linux) plus proprietary
 control-plane code (rule compiler, connections model, web backend, etc.).

 Supported architectures: x86_64, aarch64, ppc64le, riscv64.
endef

define Package/littlesnitch/conffiles
/etc/littlesnitch/main.toml
/etc/littlesnitch/networks.toml
/etc/littlesnitch/web_ui.toml
/etc/littlesnitch/executables.toml
/etc/littlesnitch/software_update.toml
endef

# No source to configure or compile - upstream provides a static binary.
Build/Configure:=
Build/Compile:=

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(TAR) --no-same-owner -xzf $(DL_DIR)/$(PKG_SOURCE) -C $(PKG_BUILD_DIR) --strip-components=2
endef

define Package/littlesnitch/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/bin/littlesnitch $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/littlesnitch.init $(1)/etc/init.d/littlesnitch

	$(INSTALL_DIR) $(1)/etc/littlesnitch
	$(INSTALL_CONF) ./files/main.toml            $(1)/etc/littlesnitch/
	$(INSTALL_CONF) ./files/networks.toml        $(1)/etc/littlesnitch/
	$(INSTALL_CONF) ./files/web_ui.toml          $(1)/etc/littlesnitch/
	$(INSTALL_CONF) ./files/executables.toml     $(1)/etc/littlesnitch/
	$(INSTALL_CONF) ./files/software_update.toml $(1)/etc/littlesnitch/

	$(INSTALL_DIR) $(1)/etc/capabilities
	$(INSTALL_DATA) ./files/capabilities.json $(1)/etc/capabilities/littlesnitch.json
endef

$(eval $(call BuildPackage,littlesnitch))
