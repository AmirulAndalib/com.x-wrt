#
# Copyright (C) 2017-2019 Chen Minqiang <ptpt52@gmail.com>
# Copyright (C) 2019 X-WRT <dev@x-wrt.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=base-config-setting
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_MAINTAINER:=Chen Minqiang <ptpt52@gmail.com>
PKG_LICENSE:=GPL-2.0

PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/package.mk

define Package/base-config-setting/Default
  SECTION:=Applications
  CATEGORY:=X
  SUBMENU:=Base config setting for this board
  DEPENDS:=+busybox +base-files +jsonfilter
endef

define Package/base-config-setting
  $(call Package/base-config-setting/Default)
  TITLE:=base-config-setting
endef

define Package/base-config-setting/config
  source "$(SOURCE)/Config.in"
endef

define Package/base-config-setting/description
  Base config setting for this board
endef

define Package/base-config-setting/conffiles
/etc/config/base_config
endef

define Package/base-config-setting/postinst
#!/bin/sh

if [ -z "$$IPKG_INSTROOT" ]; then
  ( . /etc/uci-defaults/95-base-config-setting )
  rm -f /etc/uci-defaults/95-base-config-setting
fi

exit 0
endef

define Package/base-config-setting-ext4fs
  $(call Package/base-config-setting/Default)
  TITLE:=base-config-setting ext4fs set
  DEPENDS:=+base-config-setting \
	   +busybox +@BUSYBOX_CUSTOM \
	   +@BUSYBOX_CONFIG_STAT \
	   +kmod-fs-ext4 \
	   +tune2fs \
	   +ethtool \
	   +blockd \
	   +blkid \
	   +fdisk \
	   +gdisk \
	   +partx-utils \
	   +e2fsprogs
endef

define Package/base-config-setting-ext4fs/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/fstab.config $(1)/etc/config/fstab
	$(INSTALL_DIR) $(1)/lib/preinit
	$(INSTALL_DATA) ./files/disk_ready.preinit $(1)/lib/preinit/79_disk_ready
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/disk_ready_done.init $(1)/etc/init.d/disk_ready_done
	$(INSTALL_BIN) ./files/ethtool.init $(1)/etc/init.d/ethtool
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) ./files/diskv.sh $(1)/usr/sbin/diskv
endef

define Package/base-config-setting/install
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/base_config.config $(1)/etc/config/base_config
	$(INSTALL_DIR) $(1)/usr/share/base-config-setting
	cp -r ./files/etc $(1)/usr/share/base-config-setting/
	$(INSTALL_DATA) ./files/uci.defaults $(1)/etc/uci-defaults/95-base-config-setting
	$(SED) "s#%LUCI_LOGIN%#$(CONFIG_BASE_CONFIG_SETTING_LUCI_LOGIN)#g" $(1)/etc/uci-defaults/95-base-config-setting
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) ./files/sta_disable.sh $(1)/usr/sbin/sta_disable
	$(INSTALL_BIN) ./files/system_reset.sh $(1)/usr/sbin/system_reset
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) ./files/adv_luci_disabled $(1)/etc/adv_luci_disabled
	$(INSTALL_DIR) $(1)/lib/wifi/
	$(INSTALL_DATA) ./files/wifi-init.sh $(1)/lib/wifi/wifi-init.sh
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/wifi-init.init $(1)/etc/init.d/wifi-init
	$(INSTALL_BIN) ./files/sta_disable.init $(1)/etc/init.d/sta_disable
	$(INSTALL_BIN) ./files/usb_factory_reset.init $(1)/etc/init.d/usb_factory_reset
	$(INSTALL_BIN) ./files/auto_usb_mount.init $(1)/etc/init.d/auto_usb_mount
	$(INSTALL_DATA) ./files/auto_usb_mount.hotplug $(1)/usr/share/base-config-setting/auto_usb_mount.hotplug
	$(INSTALL_DATA) ./files/mount.hotplug $(1)/usr/share/base-config-setting/mount.hotplug
	$(INSTALL_DIR) $(1)/lib/preinit
	$(INSTALL_DATA) ./files/mnt_ready.preinit $(1)/lib/preinit/81_mnt_ready
	$(INSTALL_DIR) $(1)/etc/hotplug.d/net
	$(INSTALL_DATA) ./files/15-mgr-ipaddr $(1)/etc/hotplug.d/net/15-mgr-ipaddr
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DATA) ./files/15-mgr-ipaddr $(1)/etc/hotplug.d/iface/15-mgr-ipaddr
	$(INSTALL_DIR) $(1)/usr/share/firewall4
	$(INSTALL_DATA) ./files/output-ttl.include $(1)/usr/share/firewall4/output-ttl.include
	$(INSTALL_DIR) $(1)/usr/share/ucitrack
	$(INSTALL_DATA) ./files/ucitrack/*.json $(1)/usr/share/ucitrack/
endef

define Build/Configure
  true
endef

define Build/Prepare
  true
endef

define Build/Compile
  true
endef

$(eval $(call BuildPackage,base-config-setting))
$(eval $(call BuildPackage,base-config-setting-ext4fs))
