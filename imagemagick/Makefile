#
# Copyright (C) 2010-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=imagemagick
PKG_VERSION:=7.0.2
PKG_REVISION:=4
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/ImageMagick-$(PKG_VERSION)-$(PKG_REVISION)
PKG_SOURCE:=ImageMagick-$(PKG_VERSION)-$(PKG_REVISION).tar.xz
PKG_SOURCE_URL:=http://mirror.checkdomain.de/imagemagick/releases
PKG_MD5SUM:=3850440380dfbdc7d46904efb0a24614

PKG_FIXUP:=autoreconf
PKG_REMOVE_FILES:=aclocal.m4 m4/libtool.m4
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

PKG_INSTALL=1

define Package/imagemagick/Default
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=imagemagick
  URL:=http://image_magick.veidrodis.com/
endef

define Package/imagemagick
  $(call Package/imagemagick/Default)
  DEPENDS:=+libpthread +libltdl +libjpeg +libtiff +zlib
endef

define Package/imagemagick/description
  An image manipulation library.
endef

define Package/imagemagick-jpeg
  $(call Package/imagemagick/Default)
  TITLE+= (jpeg)
  DEPENDS:=+imagemagick
endef

define Package/imagemagick-jpeg/description
  imagemagick JPEG module
endef

define Package/imagemagick-png
  $(call Package/imagemagick/Default)
  TITLE+= (png)
  DEPENDS:=+imagemagick +libpng
endef

define Package/imagemagick-png/description
  imagemagick PNG module
endef

define Package/imagemagick-tools
  $(call Package/imagemagick/Default)
  TITLE+= (tools)
  DEPENDS:=+imagemagick
endef

define Package/imagemagick-png/description
  imagemagick tools (mogrify,convert)
endef

define Package/imagemagick-tiff
  $(call Package/imagemagick/Default)
  TITLE+= (tiff)
  DEPENDS:=+imagemagick
endef

define Package/imagemagick-tiff/description
  imagemagick TIFF module
endef


CONFIGURE_ARGS += \
	--with-modules \
	--enable-shared \
	--enable-static \
	--without-pango \
	--without-bzlib \
	--without-freetype \
	--without-fontconfig \
	--without-rsvg \
	--without-fftw \
	--without-xml \
	--without-x \
	--without-magick-plus-plus \
	--without-lzma \
	--without-lcms \
	--without-jbig \

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CC="$(TARGET_CC)" \
		CFLAGS="$(TARGET_CFLAGS) -Wall -I$(PKG_BUILD_DIR)/include/linux -I$(PKG_BUILD_DIR)/include"
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/include/* \
		$(1)/opt/include/

	$(INSTALL_DIR) $(1)/opt/lib/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/*.so* \
		$(1)/opt/lib/

	$(INSTALL_DIR) $(1)/opt/lib/pkgconfig/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/lib/pkgconfig/* \
		$(1)/opt/lib/pkgconfig/
endef

define Package/imagemagick/install
	$(INSTALL_DIR) $(1)/opt/lib/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/*.so* \
		$(1)/opt/lib/
endef

define Package/imagemagick-tools/install
	$(INSTALL_DIR) $(1)/opt/bin/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/bin/{animate,compare,composite,conjure,convert,display,identify,import,magick,magick-script,mogrify,montage,stream} \
		$(1)/opt/bin/
endef

define BuildPlugin
  define Package/imagemagick-$(1)/install
	$(INSTALL_DIR) $$(1)/opt/lib/ImageMagick-$(PKG_VERSION)/modules-Q16HDRI/coders
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/ImageMagick-$(PKG_VERSION)/modules-Q16HDRI/coders/$(1).so \
		$$(1)/opt/lib/ImageMagick-$(PKG_VERSION)/modules-Q16HDRI/coders
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/ImageMagick-$(PKG_VERSION)/modules-Q16HDRI/coders/$(1).la \
		$$(1)/opt/lib/ImageMagick-$(PKG_VERSION)/modules-Q16HDRI/coders
  endef

  $$(eval $$(call BuildPackage,imagemagick-$(1)))
endef

$(eval $(call BuildPackage,imagemagick))
$(eval $(call BuildPackage,imagemagick-tools))
$(eval $(call BuildPlugin,jpeg))
$(eval $(call BuildPlugin,png))
$(eval $(call BuildPlugin,tiff))

