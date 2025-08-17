# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2025 Jiaming Shi

include $(TOPDIR)/rules.mk

PKG_NAME:=pwru
PKG_VERSION:=1.0.10
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).zip
PKG_SOURCE_URL:=https://github.com/cilium/pwru/archive/refs/tags/v$(PKG_VERSION).zip?
PKG_HASH:=38160b9d7d326663ad6012b297a77656f86ca181db27abd38d50f9fe4d3736ab

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILE:=LICENSE
PKG_MAINTAINER:=Jiaming Shi <jiaming.shih@gmail.com>

PKG_BUILD_DEPENDS:=golang/host bpf-headers
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/cilium/pwru
GO_PKG_EXCLUDES:=test-app
GO_PKG_LDFLAGS_X:= \
	$(GO_PKG)/internal/pwru.Version=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/bpf.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/pwru/Default
  SECTION:=net
  CATEGORY:=Network
  URL:=https://github.com/cilium/pwru
endef

define Package/pwru
  $(call Package/pwru/Default)
  TITLE:=eBPF-based Linux kernel networking debugger
  # You need enable KERNEL_DEBUG_INFO_BTF, KERNEL_KPROBES and KERNEL_PERF_EVENTS
  DEPENDS:=$(GO_ARCH_DEPENDS) $(BPF_DEPENDS)
endef

define Package/pwru/description
  Packet, where are you? -- eBPF-based Linux kernel networking debugger.

  pwru is an eBPF-based tool for tracing network packets in the Linux kernel
  with advanced filtering capabilities. It allows fine-grained introspection
  of kernel state to facilitate debugging network connectivity issues.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	( \
		cd $(PKG_BUILD_DIR)/libpcap && \
		./configure \
			--disable-protochain \
			--disable-rdma \
			--disable-shared \
			--disable-usb \
			--disable-netmap \
			--disable-bluetooth \
			--disable-dbus \
			--without-libnl \
			--host=$(GNU_TARGET_NAME) && \
		make \
		CFLAGS=" -g -O2 " ; \
	)
endef

define Build/Compile
	( \
		export \
		$(GO_GENERAL_BUILD_CONFIG_VARS) \
		$(GO_PKG_BUILD_CONFIG_VARS) \
		$(GO_PKG_BUILD_VARS) \
		BPF_CLANG="$(CLANG)" \
		TARGET_GOARCH="$(GO_ARCH)" ; \
		pushd $(PKG_BUILD_DIR) && \
		go generate && \
		popd ; \
		$(call GoPackage/Build/Compile) ; \
	)
endef

define Package/pwru/install
	$(call GoPackage/Package/Install/Bin,$(1))
endef

$(eval $(call GoBinPackage,pwru))
$(eval $(call BuildPackage,pwru))
