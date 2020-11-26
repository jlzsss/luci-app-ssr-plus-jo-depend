include $(TOPDIR)/rules.mk

PKG_NAME:=dns-forwarder
PKG_VERSION:=6
PKG_RELEASE:=3

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/holmium/dnsforwarder.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=587e61ae4d75dc976f538088b715a3c8ee26c144
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-6

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz


include $(INCLUDE_DIR)/package.mk

define Package/dns-forwarder
  SECTION:=net
  CATEGORY:=Network
  TITLE:=A simple DNS forwarder
  URL:=https://github.com/holmium/dns-forwarder
  DEPENDS:=+wget +ipset +dnsmasq-full +libpthread +coreutils-base64
endef

define Package/dns-forwarder/description
Forwarding queries to customized domains (and their subdomains) to specified servers over a specified protocol (UDP or TCP). non-standard ports are supported.
endef


define Package/$(PKG_NAME)/postinst
#!/bin/sh
rm -rf /tmp/luci*
endef

define Package/dns-forwarder/conffiles
/etc/dns-forwarder/dns-forwarder.conf
/etc/dns-forwarder/china-banned
/etc/dns-forwarder/base-gfwlist.txt
/etc/dns-forwarder/userlist
endef

define Build/Prepare
	$(Build/Prepare/Default)
	$(foreach po,$(wildcard ${CURDIR}/i18n/zh-cn/*.po), \
		po2lmo $(po) $(PKG_BUILD_DIR)/$(patsubst %.po,%.lmo,$(notdir $(po)));)
endef


CONFIGURE_ARGS += --enable-downloader=wget


define Package/dns-forwarder/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/dns-forwarder $(1)/usr/bin/dns-forwarder
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/*.*.lmo $(1)/usr/lib/lua/luci/i18n/
	$(CP) ./files/* $(1)/
endef

$(eval $(call BuildPackage,dns-forwarder))
