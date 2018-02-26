    cmake_minimum_required(VERSION 3.8)
project(openconnect_cmake)

set(CMAKE_CXX_STANDARD 11)
find_package(LibXml2)
find_package(ZLIB)
find_package(GnuTLS)
find_package(PkgConfig)

pkg_check_modules(STOKEN stoken REQUIRED)
pkg_check_modules(LZ4  liblz4 REQUIRED)
pkg_check_modules(PCSC libpcsclite REQUIRED)
pkg_check_modules(PROXY libproxy-1.0  REQUIRED)
pkg_check_modules(P11 p11-kit-1 REQUIRED)
pkg_check_modules(PSKC libpskc REQUIRED)

 set(CMAKE_BUILD_TYPE Debug)
set(GSSAPI_LIBS gssapi_krb5 krb5 k5crypto com_err)
set(DEPENDENCY_INCLUDES        "${LIBXML2_INCLUDE_DIRS}"
        "${GNUTLS_INCLUDE_DIRS}"
        "${STOKEN_INCLUDE_DIRS}"
        "${PCSC_INCLUDE_DIRS}"
        "${P11_INCLUDE_DIRS}"
        "${PSKC_INCLUDE_DIRS}")
include_directories( "${PROJECT_SOURCE_DIR}"
        "${DEPENDENCY_INCLUDES}")
add_definitions( ${GNUTLS_DEFINITIONS}  -pthread)
    add_library(openconnect-s STATIC
        auth-common.c
        auth-juniper.c
        auth.c
        compat.c
        cstp.c
        digest.c
        dtls.c
        esp-seqno.c
        esp.c
        gnutls-dtls.c
        gnutls-esp.c
        gnutls.c
        gnutls.h
        gnutls_tpm.c
        gssapi.c
        http-auth.c
        http.c
        iconv.c
        library.c
        lzo.c
        lzo.h
        lzs.c
        mainloop.c
        ntlm.c
        oath.c
        oncp.c
        openconnect-internal.h
        openconnect.h
        script.c
        ssl.c
        stoken.c
        tun.c)

add_executable(openconnect
        xml.c
        version.c
        main.c)

target_link_libraries(openconnect openconnect-s
        "${LIBXML2_LIBRARIES}"
        "${ZLIB_LIBRARIES}"
        "${GNUTLS_LIBRARIES}"
        "${STOKEN_LIBRARIES}"
        "${LZ4_LIBRARIES}"
        "${PCSC_LIBRARIES}"
        "${PROXY_LIBRARIES}"
        "${P11_LIBRARIES}"
        "${PSKC_LIBRARIES}"
        "${GSSAPI_LIBS}")