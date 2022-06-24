
#
# Makefile for Sherman's aquarium
#

CC = aarch64-linux-gnu-gcc

PREFIX = /usr
GNOMEDIR = /usr

INSTALL_PATH=$(DESTDIR)/usr

# Setting up the CFLAGS and LDFLAGS variables 
CFLAGS += -Wall -Wno-strict-aliasing -O3 -DIMAGE_PATH=\"/usr/share/pixmaps/shermans\"    -I/usr/include/SDL -D_GNU_SOURCE=1 -D_REENTRANT -I/usr/include/dbus-1.0 -I/usr/lib/aarch64-linux-gnu/dbus-1.0/include -pthread -I/usr/include/gtk-2.0 -I/usr/lib/aarch64-linux-gnu/gtk-2.0/include -I/usr/include/gio-unix-2.0 -I/usr/include/cairo -I/usr/include/pango-1.0 -I/usr/include/atk-1.0 -I/usr/include/cairo -I/usr/include/pixman-1 -I/usr/include/gdk-pixbuf-2.0 -I/usr/include/libmount -I/usr/include/blkid -I/usr/include/pango-1.0 -I/usr/include/harfbuzz -I/usr/include/pango-1.0 -I/usr/include/fribidi -I/usr/include/glib-2.0 -I/usr/lib/aarch64-linux-gnu/glib-2.0/include -I/usr/include/uuid -I/usr/include/freetype2 -I/usr/include/libpng16 -I/usr/include/dbus-1.0 -I/usr/lib/dbus-1.0/include -I/usr/include/dbus-1.0 -I/usr/include/glib-2.0 -I/usr/lib/glib-2.0/include -I/usr/lib/dbus-1.0/include  
LDFLAGS_GAI = $(LDFLAGS)   
LDFLAGS_SDL = $(LDFLAGS) -L/usr/lib/aarch64-linux-gnu -lSDL  -lgtk-x11-2.0 -lgdk-x11-2.0 -lpangocairo-1.0 -latk-1.0 -lcairo -lgdk_pixbuf-2.0 -lgio-2.0 -lpangoft2-1.0 -lpango-1.0 -lgobject-2.0 -lglib-2.0 -lfontconfig -lfreetype

OBJS = fish.o background.o bubble.o draw.o bottom.o soundeffects.o images.o
OBJS_GAI = settings.o pref.o date.o clock.o matrix.o over.o thermometer.o leds.o aquarium.o tetris.o plasma.o  status_linux26.o
OBJS_SDL = screensaver.o grabscreen.o

.PHONY: all install clean  shermans install-images  install_screensaver

all:  shermans

shermans_applet: $(OBJS_GAI) $(OBJS)
	$(CC) -o shermans_applet $(OBJS) $(OBJS_GAI) $(LDFLAGS_GAI)


shermans: $(OBJS) $(OBJS_SDL)
	$(CC) -o shermans $(OBJS) $(OBJS_SDL) $(LDFLAGS_SDL) -ldbus-1 -lpthread -losso -ldbus-glib-1 -ldbus-1 -lpthread -lgobject-2.0 -lglib-2.0

install:   shermans install-images  install_screensaver

install_applet: 
	install -d $(INSTALL_PATH)/bin
	install -m 755 shermans_applet $(INSTALL_PATH)/bin
	install -d $(INSTALL_PATH)/share/pixmaps/
	install -m 644 ../shermans_icon.png $(INSTALL_PATH)/share/pixmaps
install_screensaver: shermans
	install -d $(INSTALL_PATH)/bin
	install -m 755 shermans $(INSTALL_PATH)/bin
	install -d $(INSTALL_PATH)/share/xscreensaver/config
	install -m 644 ../shermans.xml $(INSTALL_PATH)/share/xscreensaver/config

install-gnome: 
	install -d $(INSTALL_PATH)/lib/bonobo/servers/
	install -m 644 ../GNOME_shermans_appletApplet.server $(INSTALL_PATH)/lib/bonobo/servers/GNOME_shermans_appletApplet.server


install-images:
	mkdir -p $(INSTALL_PATH)/share/pixmaps/shermans
	cp -r ../aquarium/* $(INSTALL_PATH)/share/pixmaps/shermans/

install-rox:
	mkdir -p "$(DESTDIR)/Sherman's aquarium"
	rm -f "$(DESTDIR)/Sherman's aquarium/.DirIcon"
	ln -s $(INSTALL_PATH)/share/pixmaps/shermans_icon.png "$(DESTDIR)/Sherman's aquarium/.DirIcon"
	install -m 755 AppRun "$(DESTDIR)/Sherman's aquarium"
	rm -f "$(DESTDIR)/Sherman's aquarium/AppletRun"
	ln -s "/Sherman's aquarium/AppRun" "$(DESTDIR)/Sherman's aquarium/AppletRun"
	rm -f "$(DESTDIR)/Sherman's aquarium/shermans_applet"
	ln -s $(INSTALL_PATH)/bin/shermans_applet "$(DESTDIR)/Sherman's aquarium/shermans_applet"
	install -m 644 AppInfo.xml "$(DESTDIR)/Sherman's aquarium"
clean:
	rm -rf *.o .xvpics *~  shermans

# Dependencies:

aquarium.h: 	defines.h
aquarium.o: 	aquarium.c aquarium.h draw.h settings.h \
		clock.h date.h bubble.h over.h leds.h thermometer.h fish.h background.h xmms_sn.h bottom.h \
		pref.h ../config.h
background.o: 	background.c aquarium.h draw.h grabscreen.h background.h ../config.h
images.o:	images.c aquarium.h ../config.h
screensaver.o:	screensaver.c bottom.h bubble.h aquarium.h background.h fish.h soundeffects.h ../config.h
bottom.o: 	bottom.c aquarium.h draw.h bottom.h fish.h ../config.h
bubble.o:	bubble.c bubble.h aquarium.h draw.h soundeffects.h ../config.h
clock.o:	clock.c clock.h draw.h aquarium.h ../config.h
date.o:		date.c date.h aquarium.h draw.h ../config.h

draw.o:		draw.c draw.h aquarium.h ../config.h
fish.o:		fish.c fish.h draw.h aquarium.h soundeffects.h ../config.h
grabscreen.o:	grabscreen.c grabscreen.h ../config.h

leds.o:		leds.c leds.h draw.h aquarium.h ../config.h
matrix.o:	matrix.c matrix.h draw.h over.h aquarium.h ../config.h
over.o:		over.c over.h aquarium.h draw.h matrix.h plasma.h tetris.h ../config.h
plasma.o:	plasma.c plasma.h aquarium.h over.h ../config.h
pref.o:		pref.c pref.h clock.h date.h background.h soundeffects.h bottom.h bubble.h fish.h over.h \
		leds.h thermometer.h xmms_sn.h ../config.h
settings.o:	settings.c settings.h aquarium.h fish.h bubble.h date.h clock.h background.h over.h \
		leds.h bottom.h xmms_sn.h matrix.h plasma.h tetris.h ../config.h
soundeffects.o: soundeffects.c soundeffects.h aquarium.h settings.h ../config.h
status_linux24.o: status_linux24.c status_linux.h status.h defines.h ../config.h
status_linux26.o: status_linux26.c status_linux.h status.h defines.h ../config.h
status_darwin.o:status_darwin.c status_darwin.h status.h defines.h ../config.h
status_freebsd.o:status_freebsd.c status_freebsd.h status.h defines.h ../config.h
tetris.o:	tetris.c tetris.h aquarium.h over.h settings.h ../config.h
thermometer.o:	thermometer.c thermometer.h aquarium.h draw.h status.h ../config.h
xmms_sn.o: 	xmms_sn.c xmms_sn.h aquarium.h draw.h ../config.h
