cmake_minimum_required (VERSION 2.6)
project (newuoa)
set (CMAKE_C_FLAGS "-Wall -lm -O3 -g")
set (CMAKE_CXX_FLAGS "-Wall -lm -O3 -g -std=c++11")
include_directories (include)
add_library (newuoa_static STATIC src/newuoa.cpp include/newuoa.h)
set_target_properties (newuoa_static PROPERTIES
    OUTPUT_NAME newuoa
    ARCHIVE_OUTPUT_DIRECTORY lib)
add_library (newuoa_shared SHARED src/newuoa.cpp include/newuoa.h)
set_target_properties (newuoa_shared PROPERTIES
    OUTPUT_NAME newuoa
    LIBRARY_OUTPUT_DIRECTORY lib)
add_executable (example_function src/example_function.c include/newuoa.h)
set_target_properties (example_function PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY bin)
target_link_libraries (example_function newuoa_static)
add_executable (example_closure src/example_closure.cpp include/newuoa.h)
set_target_properties (example_closure PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY bin)
target_link_libraries (example_closure newuoa_static)
