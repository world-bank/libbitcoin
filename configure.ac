AC_INIT([libbitcoin], [2.0.0], [genjix@riseup.net])
AC_USE_SYSTEM_EXTENSIONS
AC_LANG(C++)
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([-Wall -Werror])
AM_PROG_AR
LT_INIT
AC_PROG_CXX
AC_PROG_LIBTOOL
AC_GNU_SOURCE
AX_CXX_COMPILE_STDCXX_11(noext,mandatory)

m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AC_ARG_ENABLE(leveldb,
    [AC_HELP_STRING([--enable-leveldb],
        [Build LevelDB support.])],
    [bld_leveldb="$enable_leveldb"], [bld_leveldb="no"])
AM_CONDITIONAL([DO_LEVELDB], [test "$enable_leveldb" = "yes"])

AC_MSG_CHECKING(--enable-testnet argument)
AC_ARG_ENABLE(testnet,
    [  --enable-testnet        Enable testnet],
    [enable_testnet=$enableval],
    [enable_testnet="no"])
AC_MSG_RESULT($enable_testnet)
if test "$enable_testnet" = "yes"; then
  AC_DEFINE(ENABLE_TESTNET)
fi

AX_BOOST_BASE
#AX_BOOST_SYSTEM
#AX_BOOST_THREAD

PKG_PROG_PKG_CONFIG

if test "$bld_leveldb" = "yes"; then
    AC_SUBST(LDFLAG_LEVELDB)
    LDFLAG_LEVELDB="-lleveldb"
    AC_SUBST(CFLAG_LEVELDB)
    CFLAG_LEVELDB="-DLEVELDB_ENABLED"
fi

AM_CXXFLAGS="-ggdb -g3 -Wall -Wno-missing-braces -pedantic -Wextra -fstack-protector-all -DDEBUG -fvisibility=hidden -fvisibility-inlines-hidden"
AC_SUBST([AM_CXXFLAGS])

# Allow libsecp256k1 to be installed in a custom path:
AC_ARG_WITH([libsecp256k1],
            [AS_HELP_STRING([--with-libsecp256k1],
                            [Specify libsecp256k1 prefix])],
            [bc_search_libsecp256k1="yes"],
            [])
if test "x$bc_search_libsecp256k1" = "xyes"; then
    if test -r "${with_libsecp256k1}/include/secp256k1.h"; then
        CXXFLAGS="-I${with_libsecp256k1}/include ${CXXFLAGS}"
        LDFLAGS="-L${with_libsecp256k1}/lib ${LDFLAGS}"
    fi
fi

PKG_CHECK_MODULES([SSL], [libssl >= 0.9])
PKG_CHECK_MODULES([CURL], [libcurl])

AC_ARG_WITH([pkgconfigdir], AS_HELP_STRING([--with-pkgconfigdir=PATH],
    [Path to the pkgconfig directory [[LIBDIR/pkgconfig]]]),
    [pkgconfigdir="$withval"], [pkgconfigdir='${libdir}/pkgconfig'])
AC_SUBST([pkgconfigdir])

AC_CONFIG_FILES([Makefile include/bitcoin/Makefile src/Makefile libbitcoin.pc])
AC_OUTPUT

