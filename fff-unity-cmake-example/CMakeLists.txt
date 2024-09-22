cmake_minimum_required(VERSION 3.1)
include(CMakePrintHelpers)

project("FFF - Unity Sample Project" C)

set(TARGET_GROUP test CACHE STRING "Group to build")

# Configure wether libraries will be static or shared linked
set(BUILD_SHARED_LIBS OFF)

# Determine minimum C
set(CMAKE_C_STANDARD 11)

# Show all warning messages
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra")

add_subdirectory(src)

if(TARGET_GROUP STREQUAL test)

    # Enable debugging
    set(CMAKE_BUILD_TYPE Debug)
    set_property(GLOBAL PROPERTY CTEST_TARGETS_ADDED 1)
    
    include(CTest)
    enable_testing()

    add_subdirectory(tests)

else()
    message(FATAL_ERROR "Given TARGET_GROUP unknown")
endif()