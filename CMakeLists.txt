cmake_minimum_required(VERSION 3.2)

project(shairplay LANGUAGES C)

set(shairplay_VERSION 0.1)

if(MSVC)
  set(CMAKE_DEBUG_POSTFIX "d")
endif()


add_library(shairplay
  include/shairplay/dnssd.h
  include/shairplay/raop.h
  src/lib/raop_rtp.h
  src/lib/rsakey.c
  src/lib/rsakey.h
  src/lib/rsapem.c
  src/lib/rsapem.h
  src/lib/sdp.c
  src/lib/sdp.h
  src/lib/sockets.h
  src/lib/threads.h
  src/lib/utils.c
  src/lib/utils.h
  src/lib/base64.c
  src/lib/base64.h
  src/lib/compat.h
  src/lib/digest.c
  src/lib/digest.h
  src/lib/dnssd.c
  src/lib/dnssdint.h
  src/lib/global.h
  src/lib/http_parser.c
  src/lib/http_parser.h
  src/lib/http_request.c
  src/lib/http_request.h
  src/lib/http_response.c
  src/lib/http_response.h
  src/lib/httpd.c
  src/lib/httpd.h
  src/lib/logger.c
  src/lib/logger.h
  src/lib/memalign.h
  src/lib/netutils.c
  src/lib/netutils.h
  src/lib/raop.c
  src/lib/raop_buffer.c
  src/lib/raop_buffer.h
  src/lib/raop_rtp.c
  src/lib/alac/stdint_win.h
  src/lib/alac/alac.c
  src/lib/alac/alac.h
  src/lib/crypto/hmac.c
  src/lib/crypto/md5.c
  src/lib/crypto/os_port.h
  src/lib/crypto/rc4.c
  src/lib/crypto/sha1.c
  src/lib/crypto/aes.c
  src/lib/crypto/bigint.c
  src/lib/crypto/bigint.h
  src/lib/crypto/bigint_impl.h
  src/lib/crypto/config.h
  src/lib/crypto/crypto.h
)
target_include_directories(
  shairplay PRIVATE
  $<BUILD_INTERFACE:src/lib;include/;include/shairplay/;src/lib/crypto/;src/lib/alac/>
  INTERFACE
  $<INSTALL_INTERFACE:include/shairplay>
)

target_compile_definitions(shairplay
  PRIVATE
  _CRT_SECURE_NO_WARNINGS
  _CRT_NONSTDC_NO_DEPRECATE
  DLL_EXPORT
)

target_link_libraries(shairplay PRIVATE ws2_32.lib winmm.lib)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/shairplay-config-version.cmake
  VERSION ${shairplay_VERSION}
  COMPATIBILITY AnyNewerVersion
)

install(TARGETS shairplay EXPORT shairplay
  RUNTIME DESTINATION bin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)


install(DIRECTORY
  include/shairplay
  DESTINATION include)

install(EXPORT shairplay
  FILE
    shairplay.cmake
  NAMESPACE
    shairplay::
  DESTINATION
    lib/cmake/shairplay
)
install(
  FILES
    cmake/shairplay-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/shairplay-config-version.cmake
  DESTINATION
    lib/cmake/shairplay
)

if(MSVC AND BUILD_SHARED_LIBS)
  install(FILES
    $<TARGET_PDB_FILE:shairplay>
    DESTINATION lib
  )
endif()