#
# Copyright (C) 2006-2023 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=msd_lite
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2025-05-02
PKG_SOURCE_URL:=https://github.com/rozhuk-im/msd_lite.git
PKG_SOURCE_VERSION:=79a6c62c8fced6128a5e445ee110709b3b51bb78
# 2026-08-06 修复: 上游 submodule(liblcb) 内容/打包环境变化导致旧 hash 过期,
# CI 实际打包产物 sha256=533a2d...(两次独立验证), 旧值 bda05f... 每次 download 均 mismatch
PKG_MIRROR_HASH:=533a2d08722d41790834252196cd0fb66296c48602ed24cb8cdfd9fee0d47249

PKG_LICENSE:=BSD-2-Clause
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/msd_lite
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Program for IP TV streaming on the network via HTTP
  URL:=https://netlab.dhis.org/wiki/software:msd:lite
  DEPENDS:= +libpthread
endef

define Package/msd_lite/description
  msd_lite - Multi stream daemon lite. This lightweight version of 
  Multi Stream daemon (msd) Program for organizing IP TV streaming 
  on the network via HTTP.
endef

define Package/msd_lite/conffiles
/etc/msd_lite/msd_lite.conf
endef

# 2026-08-06 二次修复: 上游 CMakeLists install(CODE "FILE(MAKE_DIRECTORY ${CONFDIR})")
# 对绝对路径 CONFDIR=/etc/msd_lite 不走 DESTDIR 前缀, runner /etc 只读 -> Permission denied
# (download hash 修复后暴露的编译期 bug)。patches/001-fix-install-dir.patch 将其改为
# install(DIRECTORY DESTINATION ...) 受 DESTDIR 控制; /etc/msd_lite 内容由下方 install 段负责
CMAKE_OPTIONS += \
	-DCONFDIR=/etc/msd_lite

define Package/msd_lite/install
	$(INSTALL_DIR) $(1)/etc/msd_lite $(1)/usr/bin
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/msd_lite.conf $(1)/etc/msd_lite/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/msd_lite $(1)/usr/bin/
endef

$(eval $(call BuildPackage,msd_lite))
