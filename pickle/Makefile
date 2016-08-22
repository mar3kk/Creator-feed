# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pickle
PKG_VERSION=4
PKG_RELEASE:=0c
PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE).tar.bz2
PKG_SOURCE_URL:=https://github.com/datachi7d/openwrt-pickle
PKG_SOURCE_RELEASE:=$(PKG_RELEASE)
PKG_SOURCE_VERSION:=Ci40
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/pickle
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=Microchip PIC Programmer
	DEPENDS:=+kmod-bit-bang-gpio
endef

define Package/pickle/description
	Darron Broad Microchip PIC Programmer 'pickle'
	Only LVP PICs are supported
endef


TARGET_CFLAGS+= -I$(STAGING_DIR)/usr/include/libftdi1 \

define Build/Prepare
	tar -C $(PKG_BUILD_DIR) -xvjf $(DL_DIR)/$(PKG_SOURCE) --strip 1
endef

define Build/Compile
	$(CP) $(PKG_BUILD_DIR)/pickle/src/* $(PKG_BUILD_DIR)/
	mv $(PKG_BUILD_DIR)/pickle $(PKG_BUILD_DIR)/pickle_sources
	$(MAKE) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -std=gnu99 -DP32 -DPIO -DPCTRL -DPTEST -DBITBANG -DTTY -DQSORT" \
		build
endef

define Package/pickle/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pickle $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pio $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/root/.pickle
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pickle_sources/config/RIPE_06_000201.hex $(1)/root/.pickle
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pickle_sources/config/RIPE_11_000301.hex $(1)/root/.pickle
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pickle_sources/config/config* $(1)/root/.pickle
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/pickle_sources/config/*.sh $(1)/root/.pickle
	$(INSTALL_DIR) $(1)/etc/init.d
	$(CP) $(PKG_BUILD_DIR)/pickle_sources/files/etc/init.d/* $(1)/etc/init.d
endef

define Package/pickle/postinst
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/p12
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/p14
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/p16
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/p24
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/p32
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/pctrl
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/pload
	ln -sf /usr/bin/pickle $${IPKG_INSTROOT}/usr/bin/ptest
endef


$(eval $(call BuildPackage,pickle))
