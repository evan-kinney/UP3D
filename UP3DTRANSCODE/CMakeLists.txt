cmake_minimum_required(VERSION 3.5)

# Set compiler options
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Ofast")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fwhole-program -flto")

# Build the up3dtranscode executable
file(GLOB SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/*.c")
add_executable(up3dtranscode ${SOURCES} ${SOURCES_COMMON})
target_include_directories(up3dtranscode PRIVATE ${SOURCES_COMMON_DIR})
target_link_libraries(up3dtranscode m ${LIBUSB_LIBRARIES})

# Strip executables
add_custom_command(TARGET up3dtranscode POST_BUILD
    COMMAND strip $<TARGET_FILE:up3dtranscode>
)