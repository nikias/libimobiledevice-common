#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.64)
AC_INIT([libimobiledevice-glue], [1.0.0], [https://github.com/libimobiledevice/libimobiledevice-glue/issues],, [https://libimobiledevice.org])
AM_INIT_AUTOMAKE([dist-bzip2 no-dist-gzip check-news])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES])
AC_CONFIG_SRCDIR([src/])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

dnl libtool versioning
# +1 : 0 : +1  == adds new functions to the interface
# +1 : 0 : 0   == changes or removes functions (changes include both
#                 changes to the signature and the semantic)
#  ? :+1 : ?   == just internal changes
# CURRENT : REVISION : AGE
LIBIMOBILEDEVICE_GLUE_SO_VERSION=0:0:0

dnl Minimum package versions
LIBPLIST_VERSION=2.2.0

AC_SUBST(LIBIMOBILEDEVICE_GLUE_SO_VERSION)
AC_SUBST(LIBPLIST_VERSION)

# Checks for programs.
AC_PROG_CC
#AC_PROG_CXX
AM_PROG_CC_C_O
AC_PROG_LIBTOOL

# Checks for libraries.
PKG_CHECK_MODULES(libplist, libplist-2.0 >= $LIBPLIST_VERSION)

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([stdint.h stdlib.h string.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_CHECK_FUNCS([asprintf strcasecmp strdup strerror strndup stpcpy vasprintf getifaddrs])

# Check for operating system
AC_MSG_CHECKING([for platform-specific build settings])
case ${host_os} in
  *mingw32*|*cygwin*)
    AC_MSG_RESULT([${host_os}])
    win32=true
    AC_DEFINE(WINVER, 0x0501, [minimum Windows version])
    ;;
  darwin*)
    AC_MSG_RESULT([${host_os}])
    ;;
  *)
    AC_MSG_RESULT([${host_os}])
    AX_PTHREAD([], [AC_MSG_ERROR([pthread is required to build $PACKAGE_NAME])])
    AC_CHECK_LIB(pthread, [pthread_once], [], [AC_MSG_ERROR([pthread with pthread_once required to build $PACKAGE_NAME])])
    ;;
esac
AM_CONDITIONAL(WIN32, test x$win32 = xtrue)

AC_CHECK_MEMBER(struct dirent.d_type, AC_DEFINE(HAVE_DIRENT_D_TYPE, 1, [define if struct dirent has member d_type]),, [#include <dirent.h>])

AS_COMPILER_FLAGS(GLOBAL_CFLAGS, "-Wall -Wextra -Wmissing-declarations -Wredundant-decls -Wshadow -Wpointer-arith -Wwrite-strings -Wswitch-default -Wno-unused-parameter -fsigned-char -fvisibility=hidden")
AC_SUBST(GLOBAL_CFLAGS)

case "$GLOBAL_CFLAGS" in
  *-fvisibility=hidden*)
    AC_DEFINE([HAVE_FVISIBILITY], [1], [Define if compiled with -fvisibility=hidden])
esac

# check for large file support
AC_SYS_LARGEFILE

m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

AC_OUTPUT([
Makefile
src/Makefile
src/libimobiledevice-glue-1.0.pc
include/Makefile
])

echo "
Configuration for $PACKAGE $VERSION:
-------------------------------------------

  Install prefix: .........: $prefix

  Now type 'make' to build $PACKAGE $VERSION,
  and then 'make install' for installation.
"
