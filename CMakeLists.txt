cmake_minimum_required(VERSION 4.0)

project(stb
   VERSION         "1.0"
   DESCRIPTION     "stb single-file public domain libraries for C/C++"
   HOMEPAGE_URL    "https://github.com/nothings/stb"
   LANGUAGES       "C"
)

include(GNUInstallDirs)
set(STB_LIB_NAME stb)

add_library(stb source/stb_image.c)
add_library(stb::stb ALIAS stb)
target_compile_options(stb PRIVATE -Wno-unused-result)

target_include_directories(stb PUBLIC
   $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>
   $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/stb.pc.in ${CMAKE_CURRENT_BINARY_DIR}/stb.pc @ONLY)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/stb.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)

install(
   DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/
   DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/stb
)

install(
   TARGETS stb
   EXPORT stbConfig
   LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
   ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
   RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(
   EXPORT stbConfig
   NAMESPACE stb::
   DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb
)
