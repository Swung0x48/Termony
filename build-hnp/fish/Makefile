all: download/fish-3.7.1.tar.xz
	rm -rf temp build
	mkdir -p temp build
	cd temp && tar xvf ../download/fish-3.7.1.tar.xz
	# TCSANOW does not work, use TCSADRAIN
	sed -i.bak 's/TCSANOW,/TCSADRAIN,/' temp/fish-3.7.1/src/reader.cpp
	sed -i.bak 's/TCSANOW,/TCSADRAIN,/' temp/fish-3.7.1/src/fish_key_reader.cpp
	sed -i.bak 's/TCSANOW,/TCSADRAIN,/' temp/fish-3.7.1/src/builtins/fg.cpp
	cd temp/fish-3.7.1 && cat ../../0001-fix-cmake.diff | patch -Np1
	cd temp/fish-3.7.1 && cat ../../0002-fix-getpwuid.diff | patch -Np1
	cd temp/fish-3.7.1 && mkdir build
	cd temp/fish-3.7.1/build && PKG_CONFIG=/usr/bin/false cmake ../ -DSYS_PCRE2_LIB="$(shell pwd)/../sysroot/lib/libpcre2-32.so" -DSYS_PCRE2_INCLUDE_DIR="$(shell pwd)/../sysroot/include" -DCMAKE_DISABLE_FIND_PACKAGE_Curses=ON -DCURSES_HAVE_CURSES_H=ON -DCMAKE_SYSTEM_NAME=Linux -DCMAKE_SYSTEM_PROCESSOR=aarch64 -DCMAKE_C_COMPILER=$(OHOS_SDK_HOME)/native/llvm/bin/aarch64-unknown-linux-ohos-clang -DCMAKE_CXX_COMPILER=$(OHOS_SDK_HOME)/native/llvm/bin/aarch64-unknown-linux-ohos-clang++ -DCMAKE_AR=$(OHOS_SDK_HOME)/native/llvm/bin/llvm-ar -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCURSES_FOUND=ON -DCURSES_INCLUDE_DIRS="$(shell pwd)/../sysroot/include;$(shell pwd)/../sysroot/include/ncursesw" -DCURSES_LIBRARY=ncursesw -DCURSES_TINFO="$(shell pwd)/../sysroot/lib/libtinfow.so" -DCMAKE_EXE_LINKER_FLAGS="-L $(shell pwd)/../sysroot/lib" -DCMAKE_CXX_FLAGS="-DTPUTS_USES_INT_ARG=1" -DFISH_USE_SYSTEM_PCRE2=ON -DWITH_GETTEXT=OFF -DCMAKE_POLICY_VERSION_MINIMUM=3.5 -DCMAKE_INSTALL_PREFIX=/data/app/base.org/base_1.0 -DBUILD_DOCS=OFF
	cd temp/fish-3.7.1/build && make -j $(shell nproc) && make install DESTDIR=$(shell pwd)/build
	$(OHOS_SDK_HOME)/native/llvm/bin/llvm-strip build/data/app/base.org/base_1.0/bin/*
	mkdir -p ../sysroot
	# it makes hap installation fail
	rm -f build/data/app/base.org/base_1.0/share/fish/completions/..fish
	cp -rfv build/data/app/base.org/base_1.0/. ../sysroot | tee file.lst

download/fish-3.7.1.tar.xz:
	mkdir -p download
	cd download && wget https://github.com/fish-shell/fish-shell/releases/download/3.7.1/fish-3.7.1.tar.xz
