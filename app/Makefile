# Copyright 2020 Alex Woroschilow (alex.woroschilow@gmail.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
PWD:=$(shell pwd)


# modules_path = launch_dir + "/../share/flowblade/Flowblade"
# sys.path.insert(0, modules_path)

.PHONY: all

all: clean init

	mkdir --parents $(PWD)/build/Boilerplate.AppDir
	$(PWD)/bin/Apprepo.AppImage --destination=$(PWD)/build appdir boilerplate \
	    libgdk-pixbuf2.0-0 libatk1.0-0 libatk-bridge2.0-0 libgtk2.0-0 libmlt6 libmlt-data  \
	    libreadline8 at-spi2-core libpango-1.0-0 libgtk-3-0 libgirepository-1.0-1 \
		libwayland-cursor0 libselinux1 python3-mlt python3-distutils \
		python-gi python-dbus	ffmpeg python-gi-cairo	python-numpy \
		python-pil librsvg2-common gmic gir1.2-glib-2.0	gir1.2-gtk-3.0 \
		gir1.2-pango-1.0 gir1.2-gdkpixbuf-2.0 python-dbus \
		swh-plugins	frei0r-plugins

	wget --output-document="$(PWD)/build/build.deb" https://github.com/jliljebl/flowblade/releases/download/v2.8.0.2/flowblade-2.8.0.2-1_all.deb
	dpkg -x $(PWD)/build/build.deb $(PWD)/build

	cp --force --recursive $(PWD)/build/usr/lib/*			$(PWD)/build/Boilerplate.AppDir/lib64/
	cp --force --recursive $(PWD)/build/usr/share/*			$(PWD)/build/Boilerplate.AppDir/share/
	cp --force --recursive $(PWD)/build/usr/bin/*			$(PWD)/build/Boilerplate.AppDir/bin/

	echo ''                                                         >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo ''                                                         >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo '$${APPDIR}/bin/python3.8 $${APPDIR}/bin/flowblade $${@}'  >> $(PWD)/build/Boilerplate.AppDir/AppRun
		
	rm --force $(PWD)/build/Boilerplate.AppDir/*.desktop              || true
	rm --force $(PWD)/build/Boilerplate.AppDir/*.svg                  || true
	rm --force $(PWD)/build/Boilerplate.AppDir/*.png                  || true
	rm --force $(PWD)/build/Boilerplate.AppDir/lib64/libstdc++.so.6*  || true

	cp --force $(PWD)/AppDir/*.png       $(PWD)/build/Boilerplate.AppDir/      || true
	cp --force $(PWD)/AppDir/*.desktop   $(PWD)/build/Boilerplate.AppDir/      || true
	cp --force $(PWD)/AppDir/*.svg       $(PWD)/build/Boilerplate.AppDir/      || true

	export ARCH=x86_64 && $(PWD)/bin/appimagetool.AppImage $(PWD)/build/Boilerplate.AppDir $(PWD)/Flowblade.AppImage
	chmod +x $(PWD)/Flowblade.AppImage

init:
	mkdir --parents $(PWD)/bin

	wget https://apprepo.de/appimage/download/apprepo \
		--output-document=$(PWD)/bin/Apprepo.AppImage
	chmod +x $(PWD)/bin/Apprepo.AppImage

	wget https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-x86_64.AppImage \
		--output-document=$(PWD)/bin/appimagetool.AppImage
	chmod +x $(PWD)/bin/appimagetool.AppImage

clean:
	rm -rf $(PWD)/*.AppImage
	rm -rf $(PWD)/bin/*
	rm -rf $(PWD)/build
