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


add_subdirectory(lib)

if(TARGET_GROUP STREQUAL test)

    # Enable debugging
    set(CMAKE_BUILD_TYPE Debug)
    set_property(GLOBAL PROPERTY CTEST_TARGETS_ADDED 1)
    
    # Enable Coverage Tests
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fprofile-arcs -ftest-coverage")

    add_subdirectory(src)

    include(CTest)
    enable_testing()

    add_subdirectory(tests)

    # Add build target to generate Coverage Test Report to all compiled object files
    find_program(GCOV gcov)
    find_program(LCOV lcov)
    find_program(GENHTML genhtml)
    
    if(NOT GCOV)
        message(FATAL_ERROR "gcov not found!")
    endif()
    if(NOT LCOV)
        message(FATAL_ERROR "lcov not found!")
    endif()
    if(NOT GENHTML)
        message(FATAL_ERROR "genhtml not found!")
    endif()

    add_custom_target(gcov
        COMMAND mkdir -p Coverage
        DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/Testing
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )

    add_custom_command(TARGET gcov POST_BUILD
        COMMAND find ${CMAKE_BINARY_DIR} -type f -name *.c.o -exec ${GCOV} -b -c {} "\;"
        COMMAND echo "Coverage reports generated."
        COMMAND echo "You can find them on ${CMAKE_BINARY_DIR}/Coverage"
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/Coverage
        COMMENT "Coverage Reports Generated"
    )

    add_custom_target(lcov
        COMMAND mkdir -p Coverage
        DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/Testing
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )

    add_custom_command(TARGET lcov POST_BUILD
        COMMAND ${LCOV} --directory .. --capture --output-file lcov.info
        COMMAND ${GENHTML} -o . lcov.info
        COMMAND echo "Coverage reports generated."
        COMMAND echo "You can find them on ${CMAKE_BINARY_DIR}/Coverage"
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/Coverage
        COMMENT "Coverage Reports Generated"
    )

    add_custom_target(clangstaticanalysis
        COMMAND rm -f -r Staticanalysis
        COMMAND mkdir -p Staticanalysis
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )

    add_custom_command(TARGET clangstaticanalysis POST_BUILD
        COMMAND find ${CMAKE_SOURCE_DIR}/src/ -regex "\'.*\\.\\(cpp\\|hpp\\|cu\\|cuh\\|c\\|h\\)\'" -exec clang-format -style=file -dry-run {} >> clang_format.log 2>&1 "\;"
        COMMAND find ${CMAKE_SOURCE_DIR}/tests/ -regex "\'.*\\.\\(cpp\\|hpp\\|cu\\|cuh\\|c\\|h\\)\'" -exec clang-format -style=file -dry-run {} >> clang_format.log 2>&1 "\;"
        COMMAND find ${CMAKE_SOURCE_DIR}/src/ -regex "\'.*\\.\\(cpp\\|hpp\\|cu\\|cuh\\|c\\|h\\)\'" -exec clang-tidy -header-filter=''  {} -- -I${CMAKE_SOURCE_DIR}/src -I${CMAKE_SOURCE_DIR}/lib/fff -I${CMAKE_SOURCE_DIR}/lib/unity >> clang_tidy.log 2>&1 "\;"
        COMMAND find ${CMAKE_SOURCE_DIR}/tests/ -regex "\'.*\\.\\(cpp\\|hpp\\|cu\\|cuh\\|c\\|h\\)\'" -exec clang-tidy -header-filter='' {} -- -I${CMAKE_SOURCE_DIR}/src -I${CMAKE_SOURCE_DIR}/lib/fff -I${CMAKE_SOURCE_DIR}/lib/unity >> clang_tidy.log 2>&1 "\;"
        COMMAND python3 -m clang_html clang_tidy.log -o clang_tidy.html
        #COMMAND find ../../tests -regex '.*\.\(cpp\|hpp\|cu\|cuh\|c\|h\)' -exec clang-format -style=file -dry-run {} >> clang_"$(date +"%Y_%m_%d_%I_%M_%p").log" 2>&1 \;
        COMMAND echo "Static analysis report generated"
        COMMAND echo "You can find them on ${CMAKE_BINARY_DIR}/Staticanalysis"
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/Staticanalysis
        COMMENT "Static Analysis Reports Generated"
    )

else()
    message(FATAL_ERROR "Given TARGET_GROUP unknown")
endif()