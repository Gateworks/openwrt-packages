#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=openocd
PKG_SOURCE_VERSION:=170002ec2f7b416b25f20291a44dec98bb332a5c
PKG_VERSION:=$(PKG_SOURCE_VERSION)
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/Gateworks/openocd.git
PKG_MIRROR_HASH:=65e0a2e27954113a14c0031dcb1817af59cc87d11bcf7d493f7b26d1058318e5

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_MAINTAINER:=Paul Fertser <fercerpav@gmail.com>
PKG_CPE_ID:=cpe:/a:openocd:open_on-chip_debugger

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

PKG_CONFIG_DEPENDS:=CONFIG_PACKAGE_openocd_with_usb

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/openocd
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=OpenOCD Utility
  URL:=http://openocd.sf.net/
  DEPENDS:=+PACKAGE_openocd_with_usb:libusb-1.0 \
	+PACKAGE_openocd_with_usb:libusb-compat \
	+PACKAGE_openocd_with_usb:libftdi1 \
	+PACKAGE_openocd_with_usb:hidapi
endef

define Package/openocd/config
  if PACKAGE_openocd
	config PACKAGE_openocd_with_usb
		bool "Build with support for USB adapters."
		default y
  endif
endef

define Package/openocd/description
OpenOCD provides on-chip programming and debugging support with a
layered architecture of JTAG interface and TAP support including:

- (X)SVF playback to facilitate automated boundary scan and FPGA/CPLD
  programming;
- debug target support (e.g. ARM, MIPS): single-stepping,
  breakpoints/watchpoints, gprof profiling, etc;
- flash chip drivers (e.g. CFI, NAND, internal flash);
- embedded TCL interpreter for easy scripting.

Several network interfaces are available for interacting with OpenOCD:
telnet, TCL, and GDB. The GDB server enables OpenOCD to function as a
"remote target" for source-level debugging of embedded systems using
the GNU GDB program (and the others who talk GDB protocol, e.g. IDA
Pro).
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	-$(RM) $(PKG_BUILD_DIR)/guess-rev.sh
endef

CONFIGURE_ARGS += \
	--prefix="/usr" \
	--disable-werror \
	MAKEINFO=true \
	$(if $(CONFIG_PACKAGE_openocd_with_usb),,PKG_CONFIG=false) \
	--enable-dummy \
	--enable-imx_gpio \
	--enable-ftdi \
	--enable-sysfsgpio

TARGET_CFLAGS += -DRELSTR=\\\"-$(PKG_VERSION)-$(PKG_RELEASE)-OpenWrt\\\"

define Build/Compile
        +$(MAKE_VARS) \
        $(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(MAKE_PATH)
endef

define Package/openocd/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/share/openocd
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/openocd $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/openocd/scripts $(1)/usr/share/openocd
endef

$(eval $(call BuildPackage,openocd))
