#TODO: Switch to the more Conan 2.0 compliant Python way of operating it

cmake_minimum_required(VERSION 3.10)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    include("./CMakeListsConan.txt")
endif()

project(libartpalette LANGUAGES CXX)

set(LIBARTPALETTE_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../)

set(ANDROID_SOURCES
    ${LIBARTPALETTE_ROOT}/android-base/errno_restorer.h
    ${LIBARTPALETTE_ROOT}/android-base/file.cpp
    ${LIBARTPALETTE_ROOT}/android-base/file.h
    ${LIBARTPALETTE_ROOT}/android-base/log_main.h
    ${LIBARTPALETTE_ROOT}/android-base/logging.h
    ${LIBARTPALETTE_ROOT}/android-base/macros.h
    ${LIBARTPALETTE_ROOT}/android-base/off64_t.h
    ${LIBARTPALETTE_ROOT}/android-base/parseint.h
    ${LIBARTPALETTE_ROOT}/android-base/stringprintf.cpp
    ${LIBARTPALETTE_ROOT}/android-base/stringprintf.h
    ${LIBARTPALETTE_ROOT}/android-base/strings.cpp
    ${LIBARTPALETTE_ROOT}/android-base/strings.h
    ${LIBARTPALETTE_ROOT}/android-base/threads.h
    ${LIBARTPALETTE_ROOT}/android-base/unique_fd.h
    ${LIBARTPALETTE_ROOT}/android-base/utf8.h
)

set(LIBARTPALETTE_SOURCES
    ${LIBARTPALETTE_ROOT}/apex/palette.cc
)

add_library(libartpalette STATIC ${LIBARTPALETTE_SOURCES} ${ANDROID_SOURCES})
target_include_directories(libartpalette PUBLIC ${LIBARTPALETTE_ROOT}/include)
target_include_directories(libartpalette PRIVATE ${LIBARTPALETTE_ROOT})

set_property(TARGET libartpalette PROPERTY CXX_STANDARD 20)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(libartpalette PRIVATE $<BUILD_INTERFACE:-Wno-unknown-attributes>)
endif()
