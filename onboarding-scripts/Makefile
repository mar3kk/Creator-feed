include $(TOPDIR)/rules.mk

PKG_NAME:=onboarding-scripts
PKG_VERSION:=0.1.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/CreatorDev/ci40-onboarding-scripts.git

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_LICENSE_FILE:=LICENSE
PKG_LICENSE:=BSD-3-Clause

CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/onboarding-scripts
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Ci40 onboarding-scripts
  DEPENDS:=+luci +libubox-lua +libcurl +luci-mod-rpc +luci-ssl-openssl +uhttpd-mod-tls +avahi-dbus-daemon +avahi-dnsconfd +letmecreate
endef

define Package/onboarding-scripts/description
  Imagination Technologies Ci40 onboarding scripts
endef

define Package/onboarding-scripts/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/etc/init.d/* $(1)/etc/init.d/

	$(INSTALL_DIR) $(1)/etc/avahi/services
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/etc/avahi/services/* $(1)/etc/avahi/services/

	$(INSTALL_DIR) $(1)/usr/lib/lua
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lua/* $(1)/usr/lib/lua/

	$(INSTALL_DIR) $(1)/www
	$(CP) $(PKG_INSTALL_DIR)/www/* $(1)/www/

endef

$(eval $(call BuildPackage,$(PKG_NAME)))
