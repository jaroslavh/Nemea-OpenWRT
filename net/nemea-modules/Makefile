#
# Copyright (C) 2006-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk


PKG_NAME:=nemea-modules
PKG_REV:=de3c61adbbbc2492cd1759956bc945d9f728a5ac
PKG_VERSION:=$(PKG_REV)
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_SUBDIR:=nemea-modules-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/CESNET/nemea-modules
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_REV).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_FIXUP:=autoreconf


PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=COPYING

PKG_MAINTAINER:=Tomas Cejka <cejkat@cesnet.cz>

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/nemea-modules/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=NEMEA
  DEPENDS:=+nemea-framework +libpthread +librt
  TITLE:=NEMEA module
  URL:=https://github.com/CESNET/nemea-modules
  MENU:=1
endef

define Package/nemea-flow_meter
	$(call Package/nemea-modules/Default)
	TITLE+=flow_meter
	DEPENDS+= +libpcap +libstdcpp
endef
define Package/nemea-logreplay
	$(call Package/nemea-modules/Default)
	TITLE+=logreplay
	DEPENDS+= +libstdcpp
endef
define Package/nemea-flowcounter
	$(call Package/nemea-modules/Default)
	TITLE+=flowcounter
endef
define Package/nemea-traffic_repeater
	$(call Package/nemea-modules/Default)
	TITLE+=traffic_repeater
endef
define Package/nemea-topn
	$(call Package/nemea-modules/Default)
	TITLE+=topn
endef
define Package/nemea-logger
	$(call Package/nemea-modules/Default)
	TITLE+=logger
endef

define Package/nemea-flow_meter/config
	source "$(SOURCE)/Config.in"
endef

TARGET_CFLAGS += \
	-ffunction-sections \
	-fdata-sections

CONFIGURE_VARS += \
	ac_cv_linux_vers=$(LINUX_VERSION) \
	ac_cv_header_libusb_1_0_libusb_h=no \
	ac_cv_netfilter_can_compile=no

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	LIBS=-lm --bindir=/usr/bin/nemea --disable-silent-rules \
	--with-build-cc="$(HOSTCC)" \
	--with-flowcachesize=$(CONFIG_NEMEA_FLOW_CACHE_SIZE)

MAKE_FLAGS += \
	CCOPT="$(TARGET_CFLAGS) -I$(BUILD_DIR)/linux/include"

define Build/Configure
	$(call Build/Configure/Default)
endef

define Package/nemea-flow_meter/install
	$(INSTALL_DIR) $(1)/usr/bin/nemea
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nemea/flow_meter $(1)/usr/bin/nemea/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/init.d/flow_meter $(1)/etc/init.d/

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/config/flow_meter $(1)/etc/config/
endef

define Package/nemea-flowcounter/install
	$(INSTALL_DIR) $(1)/usr/bin/nemea
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nemea/flowcounter $(1)/usr/bin/nemea/
endef
define Package/nemea-logreplay/install
	$(INSTALL_DIR) $(1)/usr/bin/nemea
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nemea/logreplay $(1)/usr/bin/nemea/
endef
define Package/nemea-traffic_repeater/install
	$(INSTALL_DIR) $(1)/usr/bin/nemea
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nemea/traffic_repeater $(1)/usr/bin/nemea/
endef
define Package/nemea-topn/install
	$(INSTALL_DIR) $(1)/usr/bin/nemea
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nemea/topn $(1)/usr/bin/nemea/
endef
define Package/nemea-logger/install
	$(INSTALL_DIR) $(1)/usr/bin/nemea
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nemea/logger $(1)/usr/bin/nemea/
endef

$(eval $(call BuildPackage,nemea-flow_meter))
$(eval $(call BuildPackage,nemea-flowcounter))
$(eval $(call BuildPackage,nemea-logreplay))
$(eval $(call BuildPackage,nemea-traffic_repeater))
$(eval $(call BuildPackage,nemea-topn))
$(eval $(call BuildPackage,nemea-logger))

