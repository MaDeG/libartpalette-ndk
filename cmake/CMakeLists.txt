if (POLICY CMP0048)
    cmake_policy(SET CMP0048 NEW)
endif (POLICY CMP0048)

cmake_minimum_required(VERSION 3.10)

set(CMAKE_VERBOSE_MAKEFILE ON)

# Require out-of-source builds
file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if(EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with a CMakeLists.txt file). Please make a build subdirectory. Feel free to remove CMakeCache.txt and CMakeFiles.")
endif()

# Set a default build type if none was specified
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to 'Release' as none was specified.")
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
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

set(LIBARTPALETTE_SOURCES ${LIBARTPALETTE_ROOT}/apex/palette.cc)

add_library(artpalette STATIC ${LIBARTPALETTE_SOURCES} ${ANDROID_SOURCES})
target_include_directories(artpalette PUBLIC ${LIBARTPALETTE_ROOT}/include)
target_include_directories(artpalette PRIVATE ${LIBARTPALETTE_ROOT})

set_property(TARGET artpalette PROPERTY CXX_STANDARD 20)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(artpalette PRIVATE $<BUILD_INTERFACE:-Wno-unknown-attributes>)
endif()
