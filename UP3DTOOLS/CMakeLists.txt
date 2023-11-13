cmake_minimum_required(VERSION 3.5)

# Set compiler options
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Os -D_BSD_SOURCE")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Os -Wall")

# Build the up3dload executable
add_executable(up3dload upload.c ${SOURCES_COMMON})
target_include_directories(up3dload PRIVATE ${SOURCES_COMMON_DIR})
target_link_libraries(up3dload ${LIBUSB_LIBRARIES})

# Build the up3dinfo executable
add_executable(up3dinfo upinfo.c ${SOURCES_COMMON})
target_include_directories(up3dinfo PRIVATE ${SOURCES_COMMON_DIR})
target_link_libraries(up3dinfo ${LIBUSB_LIBRARIES})

# Build the up3dshell executable
add_executable(up3dshell upshell.c ${SOURCES_COMMON})
target_include_directories(up3dshell PRIVATE ${SOURCES_COMMON_DIR})
target_link_libraries(up3dshell ${LIBUSB_LIBRARIES} ${CURSES_LIBRARIES})

# Link pthread and math libraries if needed (Linux-specific)
if (UNIX AND NOT APPLE)
    target_link_libraries(up3dload pthread m)
    target_link_libraries(up3dinfo pthread m)
    target_link_libraries(up3dshell pthread m)
endif ()

# Strip executables
add_custom_command(TARGET up3dload up3dinfo up3dshell POST_BUILD
    COMMAND strip $<TARGET_FILE:up3dload> $<TARGET_FILE:up3dinfo> $<TARGET_FILE:up3dshell>
)