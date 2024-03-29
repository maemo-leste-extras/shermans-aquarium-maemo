dnl
dnl Sherman's aquarium 3.0.1 configure script.
dnl Written by Jonas Aaberg <cja@gmx.net> 2002-2005
dnl
dnl run autoconf to generate a configure script from this file.
dnl

AC_PREREQ(2.13)
AC_INIT()

dnl **************************************************************************
dnl                 Change those things between the star lines.
dnl ***************************************************************************

dnl This is the name of your applet, low level. $(APPLET_NAME).c is the main
dnl c file. 
APPLET_NAME="shermans_applet"

dnl The version of your applet
APPLET_VERSION="3.0.1"

dnl The display name in the gnome 2 menu and for the About box.
APPLET_NICE_NAME="Sherman's aquarium"

dnl Your name and email for the "Written by " field in the about box
APPLET_AUTHOR="Jonas Aaberg, <cja@gmx.net> 2002-2005. Graphics by Jim Toomey."

dnl Change the group. (This is just for gnome)
dnl Valid groups are: 
dnl "Accessories", "Amusements", "Internet", "Multimedia" and "Utility"
APPLET_GROUP="Amusements"

dnl The name file of the icon for the menu.
APPLET_NAME_ICON="shermans_icon.png"

dnl Images to install.
APPLET_IMAGES="aquarium/*"

dnl A one line description for the gnome menu.
APPLET_DESCRIPTION_SHORT="Bringer of life."

dnl This is the text displayed in the about box.
APPLET_DESCRIPTION="Sherman's aquarium is an applet with the\ncreatures from Jim Toomey's\nwonderful comic, Sherman's Lagoon\n(www.slagoon.com)."

dnl This is for the license line in the about box.
APPLET_LICENSE="Program released under GNU GPL.\nMost graphics Copyrighted by Jim Toomey and is NOT under GNU GPL."

dnl Version of GAI that is required
APPLET_REQUIRES_GAI_VERSION="0.5.3"

dnl Require XMMS (yes or no or checkfor)
APPLET_XMMS=checkfor

dnl ****************************************************************************

with_sdl=yes
AC_MSG_CHECKING(whether to try to include Screensaver support)
AC_ARG_ENABLE(sdl, [  --disable-sdl		  Compile Sherman's aquarium without Screensaver support],[
		     if test x$withval = xyes; then
		        with_sdl=yes
		     else
			with_sdl=no
		     fi
])
if test x$with_sdl = xno; then
   AC_MSG_RESULT(no)
else
   AC_MSG_RESULT(yes)
fi


with_gai=yes
AC_MSG_CHECKING(whether to try to compile applet support )
AC_ARG_ENABLE(gai, [  --disable-gai		  Compile Sherman's aquarium without applet support],[
		     if test x$withval = xyes; then
		        with_gai=yes
		     else
			with_gai=no
		     fi
])
if test x$with_gai = xno; then
   AC_MSG_RESULT(no)
else
   AC_MSG_RESULT(yes)
fi


AC_PROG_CC
AC_SUBST(APPLET_NAME)
AC_SUBST(APPLET_VERSION)
AC_SUBST(APPLET_NICE_NAME)
AC_SUBST(APPLET_AUTHOR)
AC_SUBST(APPLET_GROUP)
AC_SUBST(APPLET_NAME_ICON)
AC_SUBST(APPLET_IMAGES)
AC_SUBST(APPLET_DESCRIPTION_SHORT)
AC_SUBST(APPLET_DESCRIPTION)
AC_SUBST(APPLET_LICENSE)

dnl OS check

AC_MSG_CHECKING(operating system)
OSTYPE=`uname -s`
AC_MSG_RESULT($OSTYPE)
HOST="UNKNOWN"
STATUS=
SYSTEM_SPECIFIC_LIBS=
case $OSTYPE in

     Linux)
	HOST="LINUX"
	MINOR=`uname -r | sed 's/\([[0-9]]*\).\([[0-9]]*\).\([[0-9]]*\)\(.*\)/\2/'`
	if test x$MINOR = x4 ; then
		STATUS="status_linux24.o"
	else
		STATUS="status_linux26.o"
	fi
	;;
     Darwin)
        HOST="DARWIN"
	STATUS="status_drawin.o"
	;;
     FreeBSD)
	HOST="FREEBSD"
	STATUS="status_freebsd.o"
	SYSTEM_SPECIFIC_LIBS="-lkvm"
	;;
     OpenBSD)
	HOST="OPENBSD"
	;;
     NetBSD)
        HOST="NETBSD"
	;;
     SunOS)
        HOST="SUNOS"
	;;
esac
AC_SUBST(HOST)
AC_SUBST(SYSTEM_SPECIFIC_LIBS)
AC_SUBST(STATUS)

INSTALL_APPLET=
GAI=no
APPLET=
DGAI=
FOUND_GAI_LIBS=no

INSTALL_GNOME=
GNOME=no


dnl gai
if test x$with_gai = xyes ; then
AC_CHECK_PROG(PKGCONF, pkg-config, true, false)

if $PKGCONF ; then
    AC_MSG_CHECKING(for gai)
    if `pkg-config --exists gai`  ; then
	AC_MSG_RESULT(yes)
	GAI_CFLAGS=`pkg-config --cflags gai`
	GAI_LIBS=`pkg-config --libs gai`
	FOUND_GAI_LIBS=yes
    else
       AC_MSG_RESULT(no,
GAI is needed for applet support! Please visit http://gai.sf.net)
    fi
else
    AC_MSG_ERROR("can't find pkg-config!")
fi

dnl gai version
if test x$FOUND_GAI_LIBS = xyes ; then

   AC_MSG_CHECKING(for gai version >= $APPLET_REQUIRES_GAI_VERSION)
   if `pkg-config gai --atleast-version $APPLET_REQUIRES_GAI_VERSION` ; then
      AC_MSG_RESULT(yes)
      DGAI=-DGAI
      GAI=yes
      APPLET=shermans_applet
      INSTALL_APPLET=install_applet
   else
      AC_MSG_RESULT(no,
      a newer version of GAI is needed! Please visit http://gai.sf.net)
      GAI=no
   fi



dnl Gnome support
if test x$GAI = xyes ; then
AC_MSG_CHECKING(if gai is compiled with gnome support)

INSTALL_GNOME=
GNOME_SERVER=

if test `pkg-config gai --variable=gnome` = yes; then
    AC_MSG_RESULT(yes)
    GNOME=yes
    AC_MSG_CHECKING([for gnome prefix])
    with_gnome_prefix=`(pkg-config --variable=prefix libpanelapplet-2.0) 2>/dev/null`
    if test $? -ne 0; then
       with_gnome_prefix='${prefix}'
    fi
    AC_MSG_RESULT([$with_gnome_prefix])

    GNOMEDIR=$with_gnome_prefix
    INSTALL_GNOME=install-gnome
    INSTALL_PATH='$(GNOMEDIR)'
    
    GNOMESERVER=`echo -n GNOME_$APPLET_NAME;echo -n "Applet.server"`
    cat >$GNOMESERVER.in << EOF
<oaf_info>
	<oaf_server iid="OAFIID:GNOME_@APPLET_NAME@Applet_Factory" type="exe" location="@GNOMEDIR@/bin/@APPLET_NAME@">
		<oaf_attribute name="repo_ids" type="stringv">
			<item value="IDL:Bonobo/GenericFactory:1.0"/>
			<item value="IDL:Bonobo/Unknown:1.0"/>
		</oaf_attribute>
		<oaf_attribute name="name" type="string" value="@APPLET_NAME@ applet Factory"/>
		<oaf_attribute name="description" type="string" value="Factory for @APPLET_NAME@ applet"/>
	</oaf_server>

	<oaf_server iid="OAFIID:GNOME_@APPLET_NAME@Applet" type="factory" location="OAFIID:GNOME_@APPLET_NAME@Applet_Factory">
		<oaf_attribute name="repo_ids" type="stringv">
			<item value="IDL:GNOME/Vertigo/PanelAppletShell:1.0"/>
			<item value="IDL:Bonobo/Control:1.0"/>
			<item value="IDL:Bonobo/Unknown:1.0"/>
		</oaf_attribute>
		<oaf_attribute name="name" type="string" value="@APPLET_NICE_NAME@"/>
		<oaf_attribute name="description" type="string" value="@APPLET_DESCRIPTION_SHORT@"/>
		<oaf_attribute name="panel:category" type="string" value="@APPLET_GROUP@"/>
		<oaf_attribute name="panel:icon" type="string" value="@APPLET_NAME_ICON@"/>
	</oaf_server>
</oaf_info>
EOF


else
    AC_MSG_RESULT(no)
    GNOME=no
    if test x$prefix = xNONE ; then
	GNOMEDIR=/usr/local/
	INSTALL_PATH=/usr/local/
    else
	GNOMEDIR=$prefix
	INSTALL_PATH=$prefix
    fi
fi 


fi

AC_SUBST(GNOMEDIR)


ROX_APPRUN=
ROX_APPINFO=
INSTALL_ROX=
ROX_PATH=
dnl ROX
AC_MSG_CHECKING(if gai knows about the ROX panel)
if test `pkg-config gai --variable=rox` = yes; then
    AC_MSG_RESULT(yes)
    ROX_APPRUN=AppRun

    cat >AppRun.in << EOF
#!/bin/sh
# Slightly modified by Jonas Aaberg <cja@gmx.net> to suit GAI applets.

PROG=@APPLET_NAME@

APP_DIR=\`dirname "\$0"\`
APP_DIR=\`cd "\$APP_DIR";pwd\`; export APP_DIR

BIN="\$APP_DIR/\$PROG"

exec "\$BIN" --gai-rox "\$@"
EOF

    ROX_APPINFO=AppInfo.xml

    cat >AppInfo.xml.in << EOF
<?xml version="1.0"?>
<AppInfo>
  <Summary>@APPLET_DESCRIPTION_SHORT@</Summary>
</AppInfo>

EOF


    INSTALL_ROX=install-rox
    ROX_PATH=`pkg-config gai --variable=roxpath`
    AC_MSG_CHECKING(for ROX path)
    AC_MSG_RESULT($ROX_PATH)
else
    AC_MSG_RESULT(no)
fi


dnl endif gai lib not found
fi

dnl endif with gai 
fi


if test x$GAI = xno ; then
    if test x$prefix = xNONE ; then
	GNOMEDIR=/usr/local/
	INSTALL_PATH=/usr/local/
    else
	GNOMEDIR=$prefix
	INSTALL_PATH=$prefix
    fi
fi 


AC_SUBST(DGAI)
AC_SUBST(GAI_CFLAGS)
AC_SUBST(GAI_LIBS)
AC_SUBST(APPLET)
AC_SUBST(INSTALL_APPLET)
AC_SUBST(INSTALL_GNOME)
AC_SUBST(INSTALL_PATH)



AC_SUBST(INSTALL_ROX)
AC_SUBST(ROX_PATH)



dnl images
if test x$APPLET_IMAGES = x; then
   INSTALL_IMAGES=
else
   INSTALL_IMAGES=install-images
fi
AC_SUBST(INSTALL_IMAGES)

dnl Setup image path and icon
if test x$GNOME = xyes ; then
   APPLET_IMAGE_PATH=$GNOMEDIR/share/pixmaps/shermans
   APPLET_ICON=$GNOMEDIR/share/pixmaps/$APPLET_NAME_ICON
else
   if test x$prefix = xNONE ; then
      APPLET_IMAGE_PATH=/usr/local/share/pixmaps/shermans
      APPLET_ICON=/usr/local/share/pixmaps/$APPLET_NAME_ICON
   else
      APPLET_IMAGE_PATH=$prefix/share/pixmaps/shermans
      APPLET_ICON=$prefix/share/pixmaps/$APPLET_NAME_ICON
   fi
fi

AC_SUBST(APPLET_IMAGE_PATH)
AC_SUBST(APPLET_ICON)


dnl SDL
SDL=no
SCREENSAVER=
SDL_CFLAGS=
SDL_LIBS=
INSTALL_SCREENSAVER=

if test x$with_sdl = xyes ; then
   AC_CHECK_PROG(SDLCONF, sdl-config, true, false)

   if $SDLCONF ; then
      sdl_version=`sdl-config --version`
      AC_MSG_CHECKING([for SDL version ($sdl_version) >= 1.2.0])
      dnl copied from yawmppp that copied it from gtk+'s aclocal.m4. These guys must love sed
      SDL_MAJOR=`sdl-config --version | \
		 sed 's/\([[0-9]]*\).\([[0-9]]*\).\([[0-9]]*\)/\1/'`
      SDL_MINOR=`sdl-config --version | \
	         sed 's/\([[0-9]]*\).\([[0-9]]*\).\([[0-9]]*\)/\2/'`
      SDL_MICRO=`sdl-config --version | \
		 sed 's/\([[0-9]]*\).\([[0-9]]*\).\([[0-9]]*\)/\3/'`
      SDL=yes

      if test $SDL_MAJOR -eq 1 ; then
	 if test $SDL_MINOR -eq 2 ; then
	     SDL=yes;
	 else
	     SDL=no;
         fi      
      else
	 SDL=no
      fi   

      if test x$SDL = xyes ; then
	 SDL_CFLAGS=`sdl-config --cflags`
   # noch here, adding proper dbus CFLAGS, not sure it is best to add those to SDL_CFLAGS.
   DBUS_CFLAGS=`pkg-config --cflags dbus-1`
   SDL_CFLAGS="$SDL_CFLAGS $DBUS_CFLAGS"
	 SDL_LIBS=`sdl-config --libs`
   # noch here, not sure it is the best way to add, but need to add -lX11
   SDL_LIBS="$SDL_LIBS -lX11"
	 SCREENSAVER=shermans
	 INSTALL_SCREENSAVER=install_screensaver
	 AC_MSG_RESULT(yes)
	 dnl gtk+ v2.2.x
	 AC_MSG_CHECKING(for gtk+ version >= 2.2.0)
	 if `pkg-config gtk+-2.0 --atleast-version 2.2.0` ; then
	    AC_MSG_RESULT(yes)
	    GTK_CFLAGS=`pkg-config --cflags gtk+-2.0`
	    GTK_LIBS=`pkg-config --libs gtk+-2.0`
	    AC_SUBST(GTK_LIBS)
	    AC_SUBST(GTK_CFLAGS)
         else
	    AC_MSG_RESULT(no)
	    AC_MSG_ERROR(GTK+ v2.2.0 or later is needed for Sherman's aquarium!)
         fi
      else
	 AC_MSG_RESULT(no)
      fi
  fi

fi

AC_SUBST(GAI_CFLAGS)
AC_SUBST(GAI_LIBS)
AC_SUBST(APPLET)
AC_SUBST(INSTALL_SCREENSAVER)

AC_SUBST(SDL_CFLAGS)
AC_SUBST(SDL_LIBS)
AC_SUBST(SCREENSAVER)

dnl XMMS
XMMS=
XMMS_CFLAGS=
XMMS_LIBS=
XMMS_OBJ=
if test x$FOUND_GAI_LIBS = xyes ; then
if test x$with_gai = xyes ; then
if test x$APPLET_XMMS = xyes ; then
   AC_CHECK_PROG(XMMS_CONFIG, xmms-config, true, false)

   if $XMMS_CONFIG ; then
      XMMS_CFLAGS=`xmms-config --cflags`
      XMMS_LIBS=`xmms-config --libs`
      XMMS="#define XMMS"
   else
      AC_MSG_ERROR("can't find xmms-config! XMMS installed?")
   fi
fi
if test x$APPLET_XMMS = xcheckfor ; then
   AC_CHECK_PROG(XMMS_CONFIG, xmms-config, true, false)

   if $XMMS_CONFIG ; then
      XMMS_CFLAGS=`xmms-config --cflags`
      XMMS_LIBS=`xmms-config --libs`
      XMMS="#define XMMS"
      XMMS_OBJ="xmms_sn.o"
   fi
fi
fi
fi

AC_SUBST(XMMS_OBJ)
AC_SUBST(XMMS_CFLAGS)
AC_SUBST(XMMS_LIBS)
AC_SUBST(XMMS)

REMARK="This file is autogenerated by the configure script."
REMARK2="So don't add things you want permanent here."
AC_SUBST(REMARK)
AC_SUBST(REMARK2)


AC_OUTPUT($GNOMESERVER $ROX_APPRUN $ROX_APPINFO Makefile config.h shermans/Makefile)

echo
echo " Sherman's aquarium v$APPLET_VERSION"
echo "==========================="
echo " - Compile Sherman's aquarium - dockapp/wmapplet:       $GAI"
echo " - Compile Sherman's aquarium - Gnome 2 Panel applet:   $GNOME"
echo " - Compile Sherman's aquarium - Screensaver:            $SDL"
