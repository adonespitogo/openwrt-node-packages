# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_SCOPE:=node-wot
PKG_NPM_NAME:=binding-opcua
PKG_NAME:=$(PKG_NPM_SCOPE)-$(PKG_NPM_NAME)
PKG_VERSION:=0.7.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=https://registry.npmjs.org/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME)/-/
PKG_HASH:=041c7a48b6df4e3b0521921aee4539562468586d5bcfc8ecccfe7945d64e553c

PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>
PKG_LICENSE:=EPL-2.0 OR W3C-20150513
PKG_LICENSE_FILES:=

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/node-wot-binding-opcua
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=OPC UA Client Protocol Binding
  URL:=https://www.npmjs.com/package/@node-wot/binding-opcua
  DEPENDS:=+node +node-wot-core
endef

define Package/node-wot-binding-opcua/description
 W3C Web of Things (WoT) Protocol Binding for OPC UA
endef

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_target_arch=$(NODEJS_CPU) \
	npm_config_build_from_source=true \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_prefix=$(PKG_INSTALL_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	npm install -g $(PKG_BUILD_DIR)
	rm -rf $(TMP_DIR)/npm-tmp-$(TMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(TMPNPM)
endef

define Package/node-wot-binding-opcua/install
	$(INSTALL_DIR) $(1)/usr/lib/node/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME)
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME)/{package.json,*.md} \
		$(1)/usr/lib/node/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME)
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME)/{node_modules,dist} \
		$(1)/usr/lib/node/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME)
	$(INSTALL_DIR) $(1)/usr/lib/node_modules/@$(PKG_NPM_SCOPE)
	$(LN) ../../node/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME) \
		$(1)/usr/lib/node_modules/@$(PKG_NPM_SCOPE)/$(PKG_NPM_NAME)
endef

define Package/node-wot-binding-opcua/postrm
#!/bin/sh
rm /usr/lib/node_modules/@node-wot/binding-opcua || true
rm -rf /usr/lib/node/@node-wot/binding-opcua || true
rmdir /usr/lib/node_modules/@node-wot || true
rmdir /usr/lib/node/@node-wot || true
endef

$(eval $(call BuildPackage,node-wot-binding-opcua))
