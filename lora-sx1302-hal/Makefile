#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sx1302_hal_wrt
PKG_VERSION:=2.1.1
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://codeload.github.com/LXGMAX/sx1302_hal_wrt/tar.gz/V$(PKG_VERSION)?
PKG_SOURCE:=sx1302_hal_wrt-$(PKG_VERSION).tar.gz
PKG_HASH:=55dca4619e3dea080730257169f0dffea823c6ed624c2fdf38a5591ebcf4df51

#PKG_SOURCE_PROTO:=git
#PKG_SOURCE_URL:=https://github.com/LXGMAX/sx1302_hal_wrt
#PKG_SOURCE_VERSION:=936f2b421c4d39aa0a6dea34d6be21506f92f5f2

PKG_MAINTAINER:=LXGMAX <leickwell@hotmail.com>
PKG_LICENSE_FILES:=LICENSE
#PKG_BUILD_DIR:=$(BUILD_DIR)/sx1302_hal-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/sx1302_hal_wrt
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=LoRaWAN
  TITLE:=SX1302/SX1303 HAL
  DEPENDS:=+kmod-spi-dev
endef

define Package/sx1302_hal_wrt/description
  Utilities for SX1302/SX1303 Hardware Abstraction Layer
endef

CFLAGS = $(TARGET_CFLAGS)

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/libtools \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(ARCH)"

	$(MAKE) -C $(PKG_BUILD_DIR)/libloragw \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(ARCH)"

	$(MAKE) -C $(PKG_BUILD_DIR)/util_boot \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(ARCH)"

	$(MAKE) -C $(PKG_BUILD_DIR)/util_chip_id \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(ARCH)"

	$(MAKE) -C $(PKG_BUILD_DIR)/util_net_downlink \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(ARCH)"

	$(MAKE) -C $(PKG_BUILD_DIR)/util_spectral_scan \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(ARCH)"

	$(MAKE) -C $(PKG_BUILD_DIR)/packet_forwarder \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(ARCH)"
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/lgw
	$(CP) $(PKG_BUILD_DIR)/libloragw/inc/loragw_* $(1)/usr/include/lgw
	$(CP) $(PKG_BUILD_DIR)/libloragw/inc/config.h $(1)/usr/include/lgw
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libloragw/libloragw.a $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libtools/libtinymt32.a $(1)/usr/lib/
endef

define Package/sx1302_hal_wrt/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/packet_forwarder/lora_pkt_fwd $(1)/usr/bin

	$(INSTALL_DIR) $(1)/etc/lora_pkt_fwd
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/packet_forwarder/global_conf.json.sx1250.* $(1)/etc/lora_pkt_fwd

	$(INSTALL_DATA) $(PKG_BUILD_DIR)/tools/reset_lgw.sh $(1)/etc/lora_pkt_fwd
	chmod +x $(1)/etc/lora_pkt_fwd/reset_lgw.sh
endef

$(eval $(call BuildPackage,sx1302_hal_wrt))
