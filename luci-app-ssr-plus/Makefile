include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-ssr-plus
PKG_VERSION:=181
PKG_RELEASE:=4

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/config
menu "Configuration"

config PACKAGE_$(PKG_NAME)_INCLUDE_V2ray_plugin
	bool "Include Shadowsocks V2ray Plugin"
	default y if i386||x86_64||arm||aarch64

config PACKAGE_$(PKG_NAME)_INCLUDE_Xray
	bool "Include Xray"
	default y if i386||x86_64||arm||aarch64

config PACKAGE_$(PKG_NAME)_INCLUDE_Trojan
	bool "Include Trojan"
	default y if i386||x86_64||arm||aarch64

config PACKAGE_$(PKG_NAME)_INCLUDE_Redsocks2
	bool "Include Redsocks2"
	default y if i386||x86_64||arm||aarch64

config PACKAGE_$(PKG_NAME)_INCLUDE_NaiveProxy
	bool "Include NaiveProxy"
	depends on !(arc||armeb||mips||mips64||powerpc)
	default n

config PACKAGE_$(PKG_NAME)_INCLUDE_V2ray
	bool "Include V2ray"
	default n

config PACKAGE_$(PKG_NAME)_INCLUDE_Kcptun
	bool "Include Kcptun"
	default n

config PACKAGE_$(PKG_NAME)_INCLUDE_ShadowsocksR_Server
	bool "Include ShadowsocksR Server"
	default y if i386||x86_64||arm||aarch64

endmenu
endef

PKG_CONFIG_DEPENDS:= \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_V2ray_plugin \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_V2ray \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_Xray \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_Trojan \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_NaiveProxy \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_Redsocks2 \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_Kcptun \
	CONFIG_PACKAGE_$(PKG_NAME)_INCLUDE_ShadowsocksR_Server

define Package/$(PKG_NAME)
 	SECTION:=luci
	CATEGORY:=LuCI
	SUBMENU:=3. Applications
	TITLE:=SS/SSR/V2Ray/Xray/Trojan/NaiveProxy/Socks5/Tun LuCI interface
	PKGARCH:=all
	DEPENDS:=+shadowsocksr-libev-alt +ipset +ip-full +iptables-mod-tproxy +dnsmasq-full +coreutils +coreutils-base64 +pdnsd-alt +wget +lua +libuci-lua \
			+microsocks +dns2socks +shadowsocks-libev-ss-local +shadowsocksr-libev-ssr-local +shadowsocks-libev-ss-redir +simple-obfs +tcping +resolveip \
			+PACKAGE_$(PKG_NAME)_INCLUDE_V2ray_plugin:v2ray-plugin \
			+PACKAGE_$(PKG_NAME)_INCLUDE_V2ray:v2ray \
			+PACKAGE_$(PKG_NAME)_INCLUDE_Xray:xray \
			+PACKAGE_$(PKG_NAME)_INCLUDE_Trojan:trojan \
			+PACKAGE_$(PKG_NAME)_INCLUDE_Trojan:ipt2socks \
			+PACKAGE_$(PKG_NAME)_INCLUDE_NaiveProxy:naiveproxy \
			+PACKAGE_$(PKG_NAME)_INCLUDE_Redsocks2:redsocks2 \
			+PACKAGE_$(PKG_NAME)_INCLUDE_Kcptun:kcptun-client \
			+PACKAGE_$(PKG_NAME)_INCLUDE_ShadowsocksR_Server:shadowsocksr-libev-server
endef

define Build/Prepare
endef

define Build/Compile
endef

define Package/$(PKG_NAME)/conffiles
	/etc/config/shadowsocksr
	/etc/ssr/
endef

define Package/$(PKG_NAME)/install
	
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci
	cp -pR ./luasrc/* $(1)/usr/lib/lua/luci

	$(INSTALL_DIR) $(1)/
	cp -pR ./root/* $(1)/
	
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	po2lmo ./po/zh-cn/ssr-plus.po $(1)/usr/lib/lua/luci/i18n/ssr-plus.zh-cn.lmo
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	( . /etc/uci-defaults/luci-ssr-plus ) && rm -f /etc/uci-defaults/luci-ssr-plus
	rm -rf /tmp/luci-indexcache
	rm -rf /tmp/luci-modulecache/*
	chmod 755 /etc/init.d/shadowsocksr >/dev/null 2>&1
	/etc/init.d/shadowsocksr enable >/dev/null 2>&1
fi
exit 0
endef

define Package/$(PKG_NAME)/prerm
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
     /etc/init.d/shadowsocksr disable
     /etc/init.d/shadowsocksr stop
fi
exit 0
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
