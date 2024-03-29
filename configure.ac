

AC_PREREQ([2.69])
AC_INIT([wolfengine], [1.0])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

dnl m4_include([m4/ax_check_openssl.m4])

AM_INIT_AUTOMAKE
LT_INIT([disable-static pic-only])

# capture user C_EXTRA_FLAGS from ./configure line, CFLAGS may hold -g -O2 even
# if user doesn't override, no way to tell
USER_C_EXTRA_FLAGS="$C_EXTRA_FLAGS"
USER_CFLAGS="$CFLAGS"

AC_PROG_CC
AC_LANG(C)

AC_CHECK_HEADERS([openssl/engine.h])

AX_CHECK_OPENSSL(
  [have_openssl=yes],
  AC_MSG_FAILURE([could not locate OpenSSL])
)

if test "x$have_openssl" = "xyes"; then
  LIBS="$LIBS $OPENSSL_LIBS"
  CFLAGS="$CFLAGS $OPENSSL_INCLUDES"
  LDFLAGS="$LDFLAGS $OPENSSL_LDFLAGS"
fi

# DEBUG
DEBUG_CFLAGS="-g -O0 -DWOLFENGINE_DEBUG"

# DEBUG
AX_DEBUG
AS_IF([test "$ax_enable_debug" = "yes"],
      [AM_CFLAGS="$DEBUG_CFLAGS $AM_CFLAGS"],
      [])


# Digest options
AC_ARG_ENABLE([hash],
    [AS_HELP_STRING([--enable-digest],[Enable use of wc_Hash API for digesting (default: enabled)])],
    [ ENABLED_HASH=$enableval ],
    [ ENABLED_HASH=yes ]
    )

if test "$ENABLED_HASH" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -DWE_USE_HASH"
fi

AC_ARG_ENABLE([sha384],
    [AS_HELP_STRING([--enable-sha384],[Enable SHA384 (default: enabled)])],
    [ ENABLED_SHA384=$enableval ],
    [ ENABLED_SHA384=$ENABLED_HASH ]
    )

if test "$ENABLED_SHA384" = "yes"
then
    ENABLED_DIGEST="yes"
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_SHA384"
fi

AC_ARG_ENABLE([sha512],
    [AS_HELP_STRING([--enable-sha512],[Enable SHA512 (default: enabled)])],
    [ ENABLED_SHA512=$enableval ],
    [ ENABLED_SHA512=$ENABLED_HASH ]
    )

if test "$ENABLED_SHA512" = "yes"
then
    ENABLED_DIGEST="yes"
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_SHA512"
fi

AC_ARG_ENABLE([sha256],
    [AS_HELP_STRING([--enable-sha256],[Enable SHA256 (default: enabled)])],
    [ ENABLED_SHA256=$enableval ],
    [ ENABLED_SHA256=yes ]
    )

if test "$ENABLED_SHA256" = "yes"
then
    ENABLED_DIGEST="yes"
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_SHA256"
    if test "$ENABLED_HASH" = "no"
    then
        AM_CFLAGS="$AM_CFLAGS -DWE_SHA256_DIRECT"
    fi
fi


# AES-GCM
AC_ARG_ENABLE([aesgcm],
    [AS_HELP_STRING([--enable-aesgcm],[Enable AES-GCM (default: enabled)])],
    [ ENABLED_AESGCM=$enableval ],
    [ ENABLED_AESGCM=yes ]
    )

if test "$ENABLED_AESGCM" = "yes"
then
    ENABLED_AESGCM="yes"
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_AESGCM"
fi


# ECC
AC_ARG_ENABLE([ecc],
    [AS_HELP_STRING([--enable-ecc],[Enable ECC (default: enabled)])],
    [ ENABLED_ECC=$enableval ],
    [ ENABLED_ECC=yes ]
    )

if test "$ENABLED_ECC" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_ECC"
fi

AC_ARG_ENABLE([evp-pkey],
    [AS_HELP_STRING([--enable-evp-pkey],[Enable ECC using EVP_PKEY (default: enabled)])],
    [ ENABLED_EVP_PKEY=$enableval ],
    [ ENABLED_EVP_PKEY=$ENABLED_ECC ]
    )

if test "$ENABLED_EVP_PKEY" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_EVP_PKEY"
fi

AC_ARG_ENABLE([ec-key],
    [AS_HELP_STRING([--enable-ec-key],[Enable ECC using EC_KEY (default: enabled)])],
    [ ENABLED_EC_KEY=$enableval ],
    [ ENABLED_EC_KEY=$ENABLED_ECC ]
    )

if test "$ENABLED_EC_KEY" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_EC_KEY"
fi

AC_ARG_ENABLE([ecdsa],
    [AS_HELP_STRING([--enable-ecdsa],[Enable EDSA (default: enabled)])],
    [ ENABLED_ECDSA=$enableval ],
    [ ENABLED_ECDSA=$ENABLED_ECC ]
    )

if test "$ENABLED_ECDSA" = "yes"
then
    if test ENABLED_ECC = "no"
    then
        AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_ECC"
    fi
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_ECDSA"
fi

AC_ARG_ENABLE([ecdh],
    [AS_HELP_STRING([--enable-ecdh],[Enable ECDH (default: enabled)])],
    [ ENABLED_ECDH=$enableval ],
    [ ENABLED_ECDH=$ENABLED_ECC ]
    )

if test "$ENABLED_ECDH" = "yes"
then
    ENABLED_ECKG="yes"
    if test ENABLED_ECC = "no"
    then
        AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_ECC"
    fi
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_ECDH"
fi

AC_ARG_ENABLE([eckg],
    [AS_HELP_STRING([--enable-eckg],[Enable EC Key Generation (default: enabled)])],
    [ ENABLED_ECKG=$enableval ],
    [ ENABLED_ECKG=$ENABLED_ECC ]
    )

if test "$ENABLED_ECKG" = "yes"
then
    if test ENABLED_ECC = "no"
    then
        AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_ECC"
    fi
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_ECKEYGEN"
fi

AC_ARG_ENABLE([p256],
    [AS_HELP_STRING([--enable-p256],[Enable EC Curve P-256 (default: enabled)])],
    [ ENABLED_EC_P256=$enableval ],
    [ ENABLED_EC_P256=$ENABLED_ECC ]
    )

if test "$ENABLED_EC_P256" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_EC_P256"
fi

AC_ARG_ENABLE([p384],
    [AS_HELP_STRING([--enable-p384],[Enable EC Curve P-384 (default: enabled)])],
    [ ENABLED_EC_P384=$enableval ],
    [ ENABLED_EC_P384=$ENABLED_ECC ]
    )

if test "$ENABLED_EC_P384" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_EC_P384"
fi


# Check enable options
if test "$ENABLED_DIGEST" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -DWE_HAVE_DIGEST"
fi
if test "$ENABLED_HASH" = "no"
then
    if test "$ENABLED_SHA384" = "yes"
    then
        AC_MSG_ERROR([cannot enable SHA-384 without enabling hash.])
    fi
    if test "$ENABLED_SHA512" = "yes"
    then
        AC_MSG_ERROR([cannot enable SHA-512 without enabling hash.])
    fi
fi
if test "$ENABLED_DH" = "yes"
then
    if test "$ENABLED_ECKG" = "no"
    then
        AC_MSG_ERROR([cannot disable EC Key Gen with ECDH enabled.])
    fi
fi


AX_HARDEN_CC_COMPILER_FLAGS

OPTION_FLAGS="$CFLAGS $CPPFLAGS $AM_CFLAGS"

LIBS="$LIBS -lwolfssl -ldl -lm"

if test "$GCC" = "yes"
then
    AM_CFLAGS="$AM_CFLAGS -Wall -Wno-unused -Wno-error=deprecated-declarations"
fi

CFLAGS="$CFLAGS $USER_C_EXTRA_FLAGS"

AC_SUBST([AM_CPPFLAGS])
AC_SUBST([AM_CFLAGS])
AC_SUBST([AM_LDFLAGS])

AC_CONFIG_FILES([Makefile])
AC_OUTPUT

# force make clean
echo "---"
echo "Running make clean..."
make clean >/dev/null 2>&1


# output config summary
echo "---"
echo "Configuration summary for $PACKAGE_NAME version $VERSION"
echo ""
echo "   * Installation prefix:        $prefix"
echo "   * System type:                $host_vendor-$host_os"
echo "   * Host CPU:                   $host_cpu"
echo "   * C Compiler:                 $CC"
echo "   * C Flags:                    $CFLAGS"
echo "   * Debug enabled:              $ax_enable_debug"
echo
echo "   Features "
echo "   * Digest:"
echo "   *  - SHA-256:                 $ENABLED_SHA256"
echo "   *  - SHA-384:                 $ENABLED_SHA384"
echo "   *  - SHA-512:                 $ENABLED_SHA512"
echo "   * AES-GCM:                    $ENABLED_AESGCM"
echo "   * ECC:                        $ENABLED_ECC"
echo "   *  - EVP_PKEY:                $ENABLED_EVP_PKEY"
echo "   *  - EC_KEY:                  $ENABLED_EC_KEY"
echo "   *  - ECDSA:                   $ENABLED_ECDSA"
echo "   *  - EC Key Gen:              $ENABLED_ECKG"
echo "   *  - ECDH:                    $ENABLED_ECDH"
echo "   *  - P-256:                   $ENABLED_EC_P256"
echo "   *  - P-384:                   $ENABLED_EC_P384"
echo ""
echo "---"

