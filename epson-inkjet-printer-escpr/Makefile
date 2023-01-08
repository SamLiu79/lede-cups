#
# Copyright (C) 2006-2009 OpenWrt.org
# Copyright (C) 2005-2016 Weijie Gao <hackpascal@gmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=epson-inkjet-printer-escpr
PKG_VERSION:=1.7.22
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-1lsb3.2.tar.gz
PKG_SOURCE_URL:=https://download3.ebz.epson.net/dsc/f/03/00/13/96/55/c6fced63098ae1ba104f11f572794fd558ffca29/$(PKG_SOURCE_URL)
PKG_MD5SUM:=81cd972dff8b4056dec3c0c9de150331

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/epson-inkjet-printer-escpr
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Epson Inkjet ESC/P-R printer driver
  DEPENDS:=+libcups +libcupsimage
  MAINTAINER:=Weijie Gao <hackpascal@gmail.com>
endef

define Package/epson-inkjet-printer-escpr/description
  Epson Inkjet ESC/P-R printer driver
endef

define Package/epson-inkjet-printer-escpr-ppds
  SECTION:=print
  CATEGORY:=Printing and Imaging
  TITLE:=PPD files for Epson Inkjet ESC/P-R printer driver
  MAINTAINER:=Weijie Gao <hackpascal@gmail.com>
endef

define Package/epson-inkjet-printer-escpr-ppds/description
  PPD files for Epson Inkjet ESC/P-R printer driver
endef

define Package/epson-inkjet-printer-escpr-ppds/config
config EPSON_INKJET_ESCPR_PPDS_COMPILE
	bool "Compile ppd files"
	depends on PACKAGE_epson-inkjet-printer-escpr-ppds
	default n
endef

CONFIGURE_ARGS += \
	--with-cupsfilterdir="/usr/lib/cups/filter" \
	--with-cupsppddir="/usr/share/ppd"

define Package/epson-inkjet-printer-escpr/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/{libescpr.so,libescpr.so.1,libescpr.so.1.0.0} $(1)/usr/lib

	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cups/filter/{epson-escpr,epson-escpr-wrapper} $(1)/usr/lib/cups/filter/

	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/epson-escpr-set-cups-filters-permission $(1)/etc/uci-defaults/
endef

define Package/epson-inkjet-printer-escpr-ppds/install
	if [ x$(CONFIG_EPSON_INKJET_ESCPR_PPDS_COMPILE) = xy ]; then \
		$(INSTALL_DIR) $(1)/usr/lib/cups/driver; \
		ppdc $(PKG_INSTALL_DIR)/usr/share/ppd/epson-inkjet-printer-escpr $(1)/usr/lib/cups/driver/epson-inkjet-printer-escpr.ppdb; \
	else \
		$(INSTALL_DIR) $(1)/usr/share/ppd/epson-inkjet-printer-escpr; \
		$(CP) -a $(PKG_INSTALL_DIR)/usr/share/ppd/epson-inkjet-printer-escpr/* $(1)/usr/share/ppd/epson-inkjet-printer-escpr; \
	fi
endef

$(eval $(call BuildPackage,epson-inkjet-printer-escpr))
$(eval $(call BuildPackage,epson-inkjet-printer-escpr-ppds))
