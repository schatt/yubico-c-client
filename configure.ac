# Written by Simon Josefsson <simon@josefsson.org>.
# Copyright (c) 2008-2013 Yubico AB
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are
# met:
#
#   * Redistributions of source code must retain the above copyright
#      notice, this list of conditions and the following disclaimer.
#
#   * Redistributions in binary form must reproduce the above
#      copyright notice, this list of conditions and the following
#      disclaimer in the documentation and/or other materials provided
#      with the distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

AC_INIT([ykclient], [2.15], [yubico-devel@googlegroups.com])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([m4])

# Library code modified:                              REVISION++
# Interfaces changed/added/removed:   CURRENT++       REVISION=0
# Interfaces added:                             AGE++
# Interfaces removed:                           AGE=0
AC_SUBST(LT_CURRENT, 8)
AC_SUBST(LT_REVISION, 5)
AC_SUBST(LT_AGE, 5)

AM_INIT_AUTOMAKE([1.11 -Wall -Werror])
AM_SILENT_RULES([yes])
AC_PROG_CC

m4_ifdef([AM_PROG_AR], [AM_PROG_AR])

AM_MISSING_PROG(HELP2MAN, help2man, $missing_dir)
AC_LIBTOOL_WIN32_DLL
AC_PROG_LIBTOOL

gl_LD_VERSION_SCRIPT
gl_VALGRIND_TESTS

PKG_CHECK_MODULES([LIBCURL], [libcurl])

AC_ARG_ENABLE([coverage],
              [AS_HELP_STRING([--enable-coverage],
                              [use Gcov to test the test suite])],
                              [],
                              [enable_cov=no])
AM_CONDITIONAL([ENABLE_COV],[test '!' "$enable_cov" = no])

AC_ARG_ENABLE([gcc-warnings],
  [AS_HELP_STRING([--enable-gcc-warnings],
		  [turn on lots of GCC warnings (for developers)])],
  [case $enableval in
     yes|no) ;;
     *)      AC_MSG_ERROR([bad value $enableval for gcc-warnings option]) ;;
   esac
   gl_gcc_warnings=$enableval],
  [gl_gcc_warnings=no]
)

if test "$gl_gcc_warnings" = yes; then
  nw="$nw -Wsystem-headers"         # Don't let system headers trigger warnings
  nw="$nw -Wpadded"                 # Struct's arenot padded
  nw="$nw -Wc++-compat"             # We don't care strongly about C++ compilers
  nw="$nw -Wtraditional"            # Warns on #elif which we use often
  nw="$nw -Wtraditional-conversion" # Too many warnings for now
  nw="$nw -Wconversion"             # Too many warnings for now
  nw="$nw -Wsuggest-attribute=pure" # Is it worth using attributes?

  gl_MANYWARN_ALL_GCC([ws])
  gl_MANYWARN_COMPLEMENT(ws, [$ws], [$nw])
  for w in $ws; do
    gl_WARN_ADD([$w])
  done

  gl_WARN_ADD([-fdiagnostics-show-option])
fi

AC_SUBST(YKCLIENT_VERSION_MAJOR, `echo $PACKAGE_VERSION | sed 's/\(.*\)\..*/\1/g'`)
AC_SUBST(YKCLIENT_VERSION_MINOR, `echo $PACKAGE_VERSION | sed 's/.*\.\(.*\)/\1/g'`)
AC_SUBST(YKCLIENT_VERSION_PATCH, 0)
AC_SUBST(YKCLIENT_VERSION_NUMBER, `printf "0x%02x%02x%02x" $YKCLIENT_VERSION_MAJOR $YKCLIENT_VERSION_MINOR $YKCLIENT_VERSION_PATCH`)

AC_CONFIG_FILES([
  Makefile
  ykclient_version.h
  tests/Makefile
])
AC_OUTPUT

AC_MSG_NOTICE([summary of build options:

  Version:          ${VERSION}
     Library:       current $LT_CURRENT revision $LT_REVISION age $LT_AGE
     Header:        major $YKCLIENT_VERSION_MAJOR minor $YKCLIENT_VERSION_MINOR patch $YKCLIENT_VERSION_PATCH number $YKCLIENT_VERSION_NUMBER
  Host type:        ${host}
  Install prefix:   ${prefix}
  Compiler:         ${CC}
  Compiler CFLAGS:  ${CFLAGS}
  Warning flags:    ${WARN_CFLAGS}
  Library types:    Shared=${enable_shared}, Static=${enable_static}
  Curl CFLAGS:      ${LIBCURL_CFLAGS}
      LDFLAGS:      ${LIBCURL_LIBS}
  Valgrind:         ${VALGRIND}
  Version script:   $have_ld_version_script
])
