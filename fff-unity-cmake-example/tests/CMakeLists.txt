add_subdirectory(../lib build)

include_directories(
    ../lib/unity/
	../lib/fff/
    ../src/)

add_executable(
	TestBitMan
	test_bit_manipulation.c)

target_include_directories(TestBitMan 
	PRIVATE ../subprojects/fff/)

target_link_libraries(
	TestBitMan
	bit_manipulation
	unity)
    
add_test(NAME TestBitMan_test COMMAND TestBitMan WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/Testing)