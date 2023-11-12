cmake_minimum_required(VERSION 3.5)
project(UP3DTOOLS)

# Check for supported operating system
if (CMAKE_SYSTEM_NAME MATCHES "Windows")
    if (NOT MSYS)
        message(FATAL_ERROR "MSYS2 is required on Windows for this project.")
    endIF()
elseif(NOT CMAKE_SYSTEM_NAME MATCHES "Linux" AND NOT CMAKE_SYSTEM_NAME MATCHES "Darwin")
    message(FATAL_ERROR "Unsupported operating system: ${CMAKE_SYSTEM_NAME}")
endif()

# Set output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Find/include libusb-1.0 and ncurses
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBUSB REQUIRED libusb-1.0)
include_directories(${LIBUSB_INCLUDE_DIRS})
link_directories(${LIBUSB_LIBRARY_DIRS})
find_package(Curses REQUIRED)

# Define common source files
set(SOURCES_COMMON_DIR ${CMAKE_CURRENT_SOURCE_DIR}/UP3DCOMMON)
file(GLOB SOURCES_COMMON "${SOURCES_COMMON_DIR}/*.c")

# Build UP3DTOOLS
add_subdirectory(UP3DTOOLS)

# Build UP3DTRANSCODE
add_subdirectory(UP3DTRANSCODE)