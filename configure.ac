# Copyright(c)'1994-2011 by The Givaro group
# This file is part of Givaro.
# Givaro is governed by the CeCILL-B license under French law
# and abiding by the rules of distribution of free software.
# see the COPYRIGHT file for more details.

AC_PREREQ(2.61)

AC_INIT([Givaro],[3.4.0],[Jean-Guillaume.Dumas@imag.fr],[givaro],
		[http://ljk.imag.fr/CASYS/LOGICIELS/givaro])
AC_CONFIG_MACRO_DIR([macros]) #il le veut alors je le lui donne...
AC_CONFIG_AUX_DIR([build-aux])
AM_INIT_AUTOMAKE([1.8 gnu no-dependencies -Wall -Wno-portability])
AM_CONFIG_HEADER([config.h])
AX_PREFIX_CONFIG_H(givaro-config.h, __GIVARO)
AC_PATH_PROG(RM, rm, $FALSE)
RM="$RM -f"


AM_MAINTAINER_MODE
AM_DEP_TRACK
AM_OUTPUT_DEPENDENCY_COMMANDS

AM_ACLOCAL_INCLUDE(macros)

# work around to fix the backward compatibility issue of automake 1.10 with 1.9 (pb with MKDIR_P)
AC_SUBST([MKDIR_P])
# AC_DISABLE_SHARED


######################################################
# Try and pass different flags according to compiler #
######################################################

# AM_CONDITIONAL(CC_IS_INTL, [test "x${CXX}" = "xicpc" -o "x${CXX}" = "xicc"])
# $ac_cv_c_compiler_gnu :
# AM_CONDITIONAL(CC_IS_GNOU, [test "x${CXX}" = "xg++" ])

AC_SUBST([DEFAULT_CFLAGS])
AC_SUBST([DEBUG_CFLAGS])
AC_SUBST([TESTS_CFLAGS])

# Give a sane default for CFLAGS and CXXFLAGS
# DEFAULT_CFLAGS="-g -O2 -Wall -Wextra -Wno-unused-parameter"
TESTS_CFLAGS="-O0"
DEFAULT_CFLAGS="-O2 -pipe" #those are CXXFLAGS
DEBUG_CFLAGS="-g -Wall"    #those are CPPFLAGS

# disable default -g -O2 CXXFLAGS
: ${CXXFLAGS=""}

#set CXX
AC_PROG_CC([gcc icc])
AC_PROG_CXX([g++ icpc icc])

if  test "x${CXX}" = "xicpc" -o "x${CXX}" = "xicc"  ; then
	DEBUG_CFLAGS="${DEBUG_CFLAGS} -Wcheck"
   # DEBUG_CFLAGS="-fast"
else
	if test "x${CXX}" = "xg++"  ; then
		DEBUG_CFLAGS="${DEBUG_CFLAGS} -Wextra -Wno-unused-parameter"
	else
		AC_MSG_ERROR([please set environment variable 'CXX' to 'g++' or 'icpc']) ;
	fi
fi


DEFAULT_CFLAGS="${DEFAULT_CFLAGS} ${DEBUG_CFLAGS}"
TESTS_CFLAGS="${TESTS_CFLAGS} ${DEBUG_CFLAGS}"


AC_HEADER_STDC

# old ones
AC_PROG_LIBTOOL
AM_PROG_LIBTOOL

# newer libtool...
LT_PREREQ([2.2])
LT_INIT

#check lib
# AC_CHECK_LIB([givaro], [main])

# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([float.h limits.h stddef.h stdlib.h string.h sys/time.h stdint.h])
# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([floor gettimeofday pow sqrt])
# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_C_INLINE
AC_HEADER_TIME
# Check machine characteristics
AC_CHECK_SIZEOF(char, 8)
AC_CHECK_SIZEOF(short, 16)
AC_CHECK_SIZEOF(int, 32)
AC_CHECK_SIZEOF(long, 32)
AC_CHECK_SIZEOF(long long, 64)
AC_CHECK_SIZEOF(__int64, 64)

# and check (not so usefull since we have stdint.h)
# Besides, it seems it breaks givaro-config.h leaving oprhaned #endif and dubious code...
# AC_TYPE_SIZE_T
# AC_TYPE_INT8_T
# AC_TYPE_INT16_T
# AC_TYPE_INT32_T
# AC_TYPE_INT64_T
# AC_TYPE_UINT8_T
# AC_TYPE_UINT16_T
# AC_TYPE_UINT32_T
# AC_TYPE_UINT64_T

# check endianness of the architecture
AC_C_BIGENDIAN(
  [AC_DEFINE(HAVE_BIG_ENDIAN, 1, [Define that architecture uses big endian storage])],
  [AC_DEFINE(HAVE_LITTLE_ENDIAN, 1, [Define that architecture uses little endian storage])],
  [])

echo "Creating symbolic link for compilation"
if test ! -L src/kernel/system/givaro  ; then ln -s . src/kernel/system/givaro  ; fi
if test ! -L src/kernel/bstruct/givaro ; then ln -s . src/kernel/bstruct/givaro ; fi
if test ! -L src/kernel/integer/givaro ; then ln -s . src/kernel/integer/givaro ; fi
if test ! -L src/kernel/memory/givaro  ; then ln -s . src/kernel/memory/givaro  ; fi
if test ! -L src/kernel/rational/givaro; then ln -s . src/kernel/rational/givaro; fi
if test ! -L src/kernel/zpz/givaro     ; then ln -s . src/kernel/zpz/givaro     ; fi
if test ! -L src/kernel/gmp++/gmp++    ; then ln -s . src/kernel/gmp++/gmp++    ; fi
if test ! -L src/library/matrix/givaro ; then ln -s . src/library/matrix/givaro ; fi
if test ! -L src/library/poly1/givaro  ; then ln -s . src/library/poly1/givaro  ; fi
if test ! -L src/library/tools/givaro  ; then ln -s . src/library/tools/givaro  ; fi
if test ! -L src/library/vector/givaro ; then ln -s . src/library/vector/givaro ; fi


# Create some useful data types of fixed, known lengths

case 1 in
  $ac_cv_sizeof_char)
     GIVARO_INT8="char";
     ;;
esac

case 2 in
  $ac_cv_sizeof_short)
     GIVARO_INT16="short";
     ;;
  $ac_cv_sizeof_int)
     GIVARO_INT16="int";
     ;;
esac

case 4 in
  $ac_cv_sizeof_short)
     GIVARO_INT32="short";
     ;;
  $ac_cv_sizeof_int)
     GIVARO_INT32="int";
     ;;
  $ac_cv_sizeof_long)
     GIVARO_INT32="long";
     ;;
esac

case 8 in
  $ac_cv_sizeof_short)
     GIVARO_INT64="short";
     ;;
  $ac_cv_sizeof_int)
     GIVARO_INT64="int";
     ;;
  $ac_cv_sizeof_long)
     GIVARO_INT64="long";
     ;;
  $ac_cv_sizeof_long_long)
     GIVARO_INT64="long long";
     ;;
  $ac_cv_sizeof___int64)
     GIVARO_INT64="__int64";
     ;;
esac

AC_DEFINE_UNQUOTED(INT8,  $GIVARO_INT8, Canonical 8-bit data type)
AC_DEFINE_UNQUOTED(INT16, $GIVARO_INT16, Canonical 16-bit data type)
AC_DEFINE_UNQUOTED(INT32, $GIVARO_INT32, Canonical 32-bit data type)
AC_DEFINE_UNQUOTED(INT64, $GIVARO_INT64, Canonical 64-bit data type)

# This is weird
#case $ac_cv_sizeof_long in
#  $ac_cv_sizeof_long_long)
#    AC_DEFINE_UNQUOTED(_DONOTUSE_longlong__,1, long long is equal to long)
#    ;;
#esac

# Feature checks
AC_LANG([C++])

LB_CHECK_GMP(,,[
echo '*******************************************************************************'
echo ' ERROR: GMP not found!'
echo
echo ' GMP version 3.1.1 or greater is required for this library to compile. Please'
echo ' make sure GMP is installed and specify its location with the option'
echo ' --with-gmp=<prefix> when running configure.'
echo '*******************************************************************************'
exit 1
])

GIV_DOC

CXXFLAGS="${GMP_CFLAGS} ${CXXFLAGS}"
LIBS="${GMP_LIBS}"
AC_SUBST(CXXFLAGS)

AC_CONFIG_FILES([
Makefile
givaro-config
givaro-makefile
src/Makefile
src/kernel/Makefile
src/kernel/system/Makefile
src/kernel/memory/Makefile
src/kernel/gmp++/Makefile
src/kernel/integer/Makefile
src/kernel/bstruct/Makefile
src/kernel/rational/Makefile
src/kernel/zpz/Makefile
src/library/Makefile
src/library/vector/Makefile
src/library/tools/Makefile
src/library/poly1/Makefile
src/library/matrix/Makefile
macros/Makefile
examples/Makefile
examples/FiniteField/Makefile
examples/Integer/Makefile
examples/Rational/Makefile
examples/Polynomial/Makefile
tests/Makefile
docs/Makefile
])
AC_OUTPUT