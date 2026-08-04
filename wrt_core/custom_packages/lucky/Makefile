# SPDX-License-Identifier: GPL-3.0-only
#
# Lucky 3.0.0 (beta8) 自定义包
# 从官方下载站 release.66666.host 拉取 3.0.0 系列二进制
# 来源: https://release.66666.host/v3.0.0beta8/3.0.0_lucky/
#

include $(TOPDIR)/rules.mk

PKG_NAME:=lucky
PKG_VERSION:=3.0.0
PKG_RELEASE:=beta8
PKGARCH:=all

# 架构映射: OpenWrt ARCH -> Lucky 文件名架构
ifeq ($(ARCH),x86_64)
	LUCKY_ARCH:=x86_64
endif
ifeq ($(ARCH),i386)
	LUCKY_ARCH:=i386
endif
ifeq ($(ARCH),aarch64)
	LUCKY_ARCH:=arm64
endif
ifeq ($(ARCH),arm)
	LUCKY_ARCH:=armv7
endif
ifeq ($(ARCH),mipsel)
	LUCKY_ARCH:=mipsle_softfloat
endif
ifeq ($(ARCH),mips)
	LUCKY_ARCH:=mips_softfloat
endif
ifeq ($(ARCH),riscv64)
	LUCKY_ARCH:=riscv64
endif

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=GDY666 <gdy666@foxmail.com>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE)

LUCKY_DOWNLOAD_URL:=https://release.66666.host/v3.0.0beta8/3.0.0_lucky/lucky_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Lucky 3.0.0 (beta8) - STUN/DDNS/Reverse Proxy/WOL
	DEPENDS:=@(i386||x86_64||arm||aarch64||mipsel||mips||riscv64)
	URL:=https://github.com/gdy666/lucky
endef

define Package/$(PKG_NAME)/description
  Lucky main functions: STUN hole punching, DDNS, socat, reverse proxy,
  wake on lan, port forward. Version 3.0.0 beta8 from official release site.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	[ ! -f $(PKG_BUILD_DIR)/$(PKG_NAME)_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz ] && \
		wget -q "$(LUCKY_DOWNLOAD_URL)" -O $(PKG_BUILD_DIR)/$(PKG_NAME)_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz
	tar -xzf $(PKG_BUILD_DIR)/$(PKG_NAME)_$(PKG_VERSION)_Linux_$(LUCKY_ARCH).tar.gz -C $(PKG_BUILD_DIR)
	chmod +x $(PKG_BUILD_DIR)/lucky
endef

define Build/Compile
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lucky $(1)/usr/bin/lucky
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/lucky.init $(1)/etc/init.d/lucky
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
