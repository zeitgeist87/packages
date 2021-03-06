#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=micropython
PKG_VERSION=1.3-20140828-$(PKG_SOURCE_VERSION)
PKG_RELEASE:=1

PKG_MAINTAINER:=Roger D <rogerdammit@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/micropython/micropython.git
PKG_SOURCE_VERSION:=110ba35980d03da2e744a8aadd85d08c0c06a23b

PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION)
PKG_SOURCE:=$(PKG_SOURCE_SUBDIR).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/micropython
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Micro Python
  URL:=http://micropython.org
  DEPENDS:=+libffi
endef

define Package/micropython/description
 This package contains Micro Python, a lean and fast implementation of the Python 3.4 programming language
 that is optimised to run on a microcontroller (and low power computers).
endef

# The following hacks have been tested for MIPS builds, but may need to be modified for other platforms.
# (Based on: https://github.com/pfalcon/micropython/blob/2663b63241498569d9acb9c7cf9e01e831dbd816/unix/build-mips.sh)
TARGET_CFLAGS_EXTRA += -DMICROPY_GCREGS_SETJMP=1 -DMICROPY_NLR_SETJMP=1 -DMICROPY_EMIT_X64=0
TARGET_STRIPFLAGS_EXTRA += --remove-section=.pdr -R .comment -R .gnu.version -R .gnu.version_r -R .gnu.attributes -R .reginfo -R .mdebug.abi32

MAKE_FLAGS += \
       -C $(PKG_BUILD_DIR)/unix \
       MICROPY_USE_READLINE=0 \
       CFLAGS_EXTRA="$(TARGET_CFLAGS_EXTRA)" \
       STRIPFLAGS_EXTRA="$(TARGET_STRIPFLAGS_EXTRA)"

define Package/micropython/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/unix/micropython $(1)/usr/bin/micropython
endef

$(eval $(call BuildPackage,micropython))
