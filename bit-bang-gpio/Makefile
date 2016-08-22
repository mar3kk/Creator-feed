#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=bit-bang-gpio
PKG_VERSION:=1
PKG_RELEASE:=0
PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE).tar.gz
PKG_SOURCE_URL:=https://github.com/datachi7d/openwrt-pickle
PKG_SOURCE_RELEASE:=$(PKG_RELEASE)
PKG_SOURCE_VERSION:=Ci40
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define KernelPackage/bit-bang-gpio
	SUBMENU:=Other modules
	TITLE:=Bit-bang GPIO
	DEPENDS:=@GPIO_SUPPORT
	FILES:=$(PKG_BUILD_DIR)/gpio-bb.ko
	KCONFIG:=
	AUTOLOAD:=$(call AutoProbe,gpio-bb.ko)
endef

define KernelPackage/bit-bang-gpio/description
	Kernel module to bit-bang GPIO
endef

EXTRA_KCONFIG:= \
	CONFIG_BIT_BANG_GPIO=m

EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \

MAKE_OPTS:= \
	ARCH="$(LINUX_KARCH)" \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	SUBDIRS="$(PKG_BUILD_DIR)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Compile
	$(CP) $(PKG_BUILD_DIR)/bit-bang-gpio/src/* $(PKG_BUILD_DIR)/
	$(MAKE) -C "$(LINUX_DIR)" \
	$(MAKE_OPTS) \
	modules
endef

define Package/postinst
	#!/bin/sh
	mknod /dev/gpio-bb c 180 0
endef

$(eval $(call KernelPackage,bit-bang-gpio))
