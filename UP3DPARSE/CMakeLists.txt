cmake_minimum_required(VERSION 3.5)
project(UP3DPARSE)

# Set output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Set compiler options
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Os")

# Build the parse executable
add_executable(parse parse.c)

# Build the convg executable
add_executable(convg convg.c)

# Strip executables
add_custom_command(TARGET parse POST_BUILD
    COMMAND strip $<TARGET_FILE:parse> $<TARGET_FILE:convg>
)