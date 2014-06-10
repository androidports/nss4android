nss4android
===========

NSS for Android

## NSPR ###

### NSPR Prebuild  ###

        rm nspr-4.10/nspr/config.cache
        cd nspr-4.10/nspr; ./configure
        make -C nspr-4.10/nspr/config/ clean
        make -C nspr-4.10/nspr/config/
        cp nspr-4.10/nspr/config/nsinstall nspr-4.10/nsinstall

### NSPR Build & Install ###

        rm nspr-4.10/nspr/config.cache
        cd nspr-4.10/nspr; PKG_CONFIG_PATH=$(PKG_CONFIG_PATH) NSINSTALL=$(BASE_DIR)/nspr-4.10/nsinstall \
          ./configure --host=$(HOST) --prefix=$(EXTLIBS_PREFIX_DIR) --with-android-ndk=$(NDK_ROOT) \
          --with-android-toolchain=$(NDK_TOOLCHAIN_DIR)

        cd nspr-4.10/nspr; make

        cd nspr-4.10/nspr; make install

## NSS ##

### NSS Prebuild ###

        make -C nss-3.15.1/nss/coreconf/nsinstall
        mkdir -p nss-3.15.1/nss/coreconf/nsinstall/Android8_arm_DBG.OBJ
        cp nss-3.15.1/nss/coreconf/nsinstall/Darwin12.5.0_DBG.OBJ/* \
        nss-3.15.1/nss/coreconf/nsinstall/Android8_arm_DBG.OBJ


### NSS Build & Install ###

        cd nss-3.15.1/nss; OS_TARGET=Android ANDROID_NDK=$(NDK_ROOT) \
          NSPR_CONFIGURE_OPTS="--with-android-toolchain=$(NDK_TOOLCHAIN_DIR)" \
          make NSPR_INCLUDE_DIR=$(LIBS_PREFIX_DIR)/include/nspr \
          USE_SYSTEM_ZLIB=1 ZLIB_LIBS=-lz NSPR_LIB_DIR=$(LIBS_PREFIX_DIR)/lib

        cd nss-3.15.1/dist; install -v Android*/lib/*.so $(LIBS_PREFIX_DIR)/lib
        cd nss-3.15.1/dist; install -v Android*/lib/*.a $(LIBS_PREFIX_DIR)/lib
        cd nss-3.15.1/dist; install -v -d $(LIBS_PREFIX_DIR)/include/nss
        cd nss-3.15.1/dist; cp -v -RL {public,private}/nss/* $(LIBS_PREFIX_DIR)/include/nss
        cd nss-3.15.1/dist; install -v Android*/bin/{certutil,nss-config,pk12util} $(LIBS_PREFIX_DIR)/bin
        cd nss-3.15.1/dist; install -v Android*/lib/pkgconfig/* $(LIBS_PREFIX_DIR)/lib/pkgconfig
