#TODO: Switch to the more Conan 2.0 compliant Python way of operating it

cmake_minimum_required(VERSION 3.10)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    include("./CMakeListsConan.txt")
endif()

project(libartpalette LANGUAGES CXX)

set(LIBARTPALETTE_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../)

set(ANDROID_SOURCES
    ${LIBZIPARCHIVE_ROOT}/android-base/errno_restorer.h
    ${LIBZIPARCHIVE_ROOT}/android-base/file.cpp
    ${LIBZIPARCHIVE_ROOT}/android-base/file.h
    ${LIBZIPARCHIVE_ROOT}/android-base/log_main.h
    ${LIBZIPARCHIVE_ROOT}/android-base/logging.h
    ${LIBZIPARCHIVE_ROOT}/android-base/macros.h
    ${LIBZIPARCHIVE_ROOT}/android-base/off64_t.h
    ${LIBZIPARCHIVE_ROOT}/android-base/parseint.h
    ${LIBZIPARCHIVE_ROOT}/android-base/stringprintf.cpp
    ${LIBZIPARCHIVE_ROOT}/android-base/stringprintf.h
    ${LIBZIPARCHIVE_ROOT}/android-base/strings.cpp
    ${LIBZIPARCHIVE_ROOT}/android-base/strings.h
    ${LIBZIPARCHIVE_ROOT}/android-base/threads.h
    ${LIBZIPARCHIVE_ROOT}/android-base/unique_fd.h
    ${LIBZIPARCHIVE_ROOT}/android-base/utf8.h
)

set(LIBARTPALETTE_SOURCES
    ${LIBARTPALETTE_ROOT}/apex/palette.cc
)

add_library(LIBARTPALETTE STATIC ${LIBARTPALETTE_SOURCES} ${ANDROID_SOURCES})
target_include_directories(LIBARTPALETTE PUBLIC ${LIBARTPALETTE_ROOT}/include)
target_include_directories(LIBARTPALETTE PRIVATE ${LIBARTPALETTE_ROOT})

set_property(TARGET LIBARTPALETTE PROPERTY CXX_STANDARD 20)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(LIBARTPALETTE PRIVATE $<BUILD_INTERFACE:-Wno-unknown-attributes>)
endif()
