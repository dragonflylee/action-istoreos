include $(TOPDIR)/rules.mk

PKG_NAME:=mediainfo
PKG_VERSION:=25.10
PKG_RELEASE:=0

PKG_SOURCE:=MediaInfo_CLI_$(PKG_VERSION)_GNU_FromSource.tar.xz
PKG_SOURCE_URL:=https://mediaarea.net/download/binary/mediainfo/$(PKG_VERSION)
PKG_HASH:=9a7743562053d619dcfd0cdafe71eadd6c36b134649d378a4c5c8d074ace42b5

PKG_LICENSE:=BSD-2-Clause
PKG_LICENSE_FILES:=MediaInfo_CLI_GNU_FromSource/MediaInfo/README.md

UNPACK_CMD=xzcat $(DL_DIR)/$(PKG_SOURCE) | $(HOST_TAR) -C $(PKG_BUILD_DIR) $(TAR_OPTIONS)  # Need to change from default because unpacked file has different folder name

CUSTOM_ARGS=--enable-static \
			--disable-shared \
			--enable-staticlibs

include $(INCLUDE_DIR)/package.mk

define Package/mediainfo
	SECTION:=multimedia
	CATEGORY:=Multimedia
	TITLE:=Provides relevant technical and tag data for video and audio files
	URL:=https://mediaarea.net/en/MediaInfo

	DEPENDS:=+zlib +libpthread +libstdcpp
endef

define Package/mediainfo/description
	mediainfo provides a convenient unified display of the most relevant technical and tag data for video and audio files.
	statically compiled
endef

define Build/Configure
	$(info	skipping configure)
endef

#Override whole process to follow the steps in MediaInfo_CLI_GNU_FromSource/CLI_Compile.sh
define Build/Compile
	cd $(PKG_BUILD_DIR)/MediaInfo_CLI_GNU_FromSource/ZenLib/Project/GNU/Library/ ; \
	./configure $(CUSTOM_ARGS) $(CONFIGURE_ARGS) ; \
	$(MAKE) $(MAKE_FLAGS) ; \
	$(MAKE) $(MAKE_INSTALL_FLAGS) install

	cd $(PKG_BUILD_DIR)/MediaInfo_CLI_GNU_FromSource/MediaInfoLib/Project/GNU/Library/ ; \
	./configure $(CUSTOM_ARGS) $(CONFIGURE_ARGS) ; \
	$(MAKE) $(MAKE_FLAGS) ; \
	$(MAKE) $(MAKE_INSTALL_FLAGS) install

	cd $(PKG_BUILD_DIR)/MediaInfo_CLI_GNU_FromSource/MediaInfo/Project/GNU/CLI/ ; \
	./configure $(CUSTOM_ARGS) $(CONFIGURE_ARGS) ; \
	$(MAKE) $(MAKE_FLAGS) ; \
	$(MAKE) $(MAKE_INSTALL_FLAGS) install
endef

define Package/mediainfo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libzen.la $(1)/usr/lib/libzen.la
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libmediainfo.la $(1)/usr/lib/libmediainfo.la

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mediainfo $(1)/usr/bin/mediainfo
endef

$(eval $(call BuildPackage,mediainfo))