# SPDX-License-Identifier: GPL-2.0-or-later
#
# Little Snitch for Linux - OpenWrt package (multi-arch)
#
# Repackages the upstream static musl binary into an apk package.
# The binary embeds eBPF objects and BTF metadata that must not be stripped.

include $(TOPDIR)/rules.mk

PKG_NAME:=littlesnitch
PKG_VERSION:=1.0.9
PKG_RELEASE:=1

PKG_MAINTAINER:=Jiaming Shi <jiaming.shih@gmail.com>
PKG_LICENSE:=Proprietary
PKG_LICENSE_FILES:=

ifeq ($(ARCH),x86_64)
  UPSTREAM_ARCH:=amd64
  PKG_HASH:=890e780f4d1cf6016ca9195fcf5535c8df0acd2bdb538721b0831a0c43016afb
else ifeq ($(ARCH),aarch64)
  UPSTREAM_ARCH:=arm64
  PKG_HASH:=5ba0584bf8b5e55b97034f4ddbee28623a49aa9e33e2615522ca27f085826c5e
else ifeq ($(ARCH),powerpc64)
  UPSTREAM_ARCH:=ppc64le
  PKG_HASH:=c612f1d3c54262677e23f6af86157b37e8a515731664cdaa6ba65537915f2bd8
else ifeq ($(ARCH),riscv64)
  UPSTREAM_ARCH:=riscv64
  PKG_HASH:=0e9181c68af186e84ea0cacb2a0b9404372e1f8829a7dc8ea6d606b2af031228
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
	$(INSTALL_CONF) ./files/main.toml        $(1)/etc/littlesnitch/
	$(INSTALL_CONF) ./files/networks.toml    $(1)/etc/littlesnitch/
	$(INSTALL_CONF) ./files/web_ui.toml      $(1)/etc/littlesnitch/
	$(INSTALL_CONF) ./files/executables.toml $(1)/etc/littlesnitch/

	$(INSTALL_DIR) $(1)/etc/capabilities
	$(INSTALL_DATA) ./files/capabilities.json $(1)/etc/capabilities/littlesnitch.json
endef

$(eval $(call BuildPackage,littlesnitch))
