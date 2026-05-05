# Use git to fetch source so we don't depend on a missing tarball
PKG_NAME:=dvb-hd2312
PKG_VERSION:=1.0
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=hanwckf

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/hanwckf/hd2312.git
PKG_SOURCE_VERSION:=59c4dbfdc504c1230948f63fe5b0a47ca6c71bf8
PKG_MIRROR_HASH:=skip

include $(INCLUDE_DIR)/package.mk

define KernelPackage/hd2312
  SUBMENU:=Other modules
  TITLE:=NERC DTV (HD2312) DTMB/DMB-TH USB adapter support
  DEPENDS:=+kmod-dvb-usb +kmod-dvb-core
  FILES:=$(PKG_BUILD_DIR)/hd2312.ko
  AUTOLOAD:=$(if $(CONFIG_PACKAGE_kmod-dvb-hd2312),hd2312)
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	# we need to copy our local src/ files over the fetched git source
	cp -f $(TOPDIR)/package/hd2312/src/* $(PKG_BUILD_DIR)/
endef

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		ARCH="$(LINUX_KARCH)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		M="$(PKG_BUILD_DIR)" \
		EXTRA_CFLAGS="-I$(PKG_BUILD_DIR)/dvb-usb -I$(PKG_BUILD_DIR)" \
		modules
endef

$(eval $(call KernelPackage,hd2312))
