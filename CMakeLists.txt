# libsrtp, following Chromium's BUILD.gn. Uses BoringSSL for crypto: needs a
# `boringssl` target (boringssl-min).
cmake_minimum_required(VERSION 3.22)
project(libsrtp C)

add_library(libsrtp STATIC
  crypto/cipher/aes_gcm_ossl.c
  crypto/cipher/aes_icm_ossl.c
  crypto/cipher/cipher.c
  crypto/cipher/cipher_test_cases.c
  crypto/cipher/null_cipher.c
  crypto/hash/auth.c
  crypto/hash/auth_test_cases.c
  crypto/hash/hmac_ossl.c
  crypto/hash/null_auth.c
  crypto/kernel/alloc.c
  crypto/kernel/crypto_kernel.c
  crypto/kernel/err.c
  crypto/kernel/key.c
  crypto/math/datatypes.c
  crypto/replay/rdb.c
  crypto/replay/rdbx.c
  srtp/srtp.c
)
# libsrtp_config (a public config in gn).
target_include_directories(libsrtp PUBLIC . config include crypto/include)
target_compile_definitions(libsrtp PUBLIC
  PACKAGE_STRING="24b3bf8f19b6f5ab4cd2bcceb4f4064e"
  PACKAGE_VERSION="24b3bf8f19b6f5ab4cd2bcceb4f4064e"
  HAVE_CONFIG_H
  OPENSSL
  GCM
  HAVE_STDLIB_H
  HAVE_STRING_H
  HAVE_STDINT_H
  HAVE_INTTYPES_H
  HAVE_INT16_T
  HAVE_INT32_T
  HAVE_INT8_T
  HAVE_UINT16_T
  HAVE_UINT32_T
  HAVE_UINT64_T
  HAVE_UINT8_T
  LIBSRTP_HAVE_CRYPTEX
)
if(WIN32)
  target_compile_definitions(libsrtp PUBLIC HAVE_WINSOCK2_H)
else()
  target_compile_definitions(libsrtp PUBLIC
    HAVE_ARPA_INET_H HAVE_NETINET_IN_H HAVE_SYS_TYPES_H HAVE_UNISTD_H)
endif()
target_link_libraries(libsrtp PRIVATE boringssl)
