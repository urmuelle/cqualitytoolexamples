cmake_minimum_required(VERSION 3.20)

project("Clang Static Analysis Example" C)

# Configure wether libraries will be static or shared linked
set(BUILD_SHARED_LIBS OFF)

# Determine minimum C
set(CMAKE_C_STANDARD 11)

# Enable debugging
set(CMAKE_BUILD_TYPE Debug)

# Show all warning messages
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra")
set_property(GLOBAL PROPERTY CTEST_TARGETS_ADDED 1)

add_subdirectory(src)
add_subdirectory(lib)