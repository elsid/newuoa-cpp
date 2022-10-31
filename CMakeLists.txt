cmake_minimum_required(VERSION 3.16)

project(newuoa VERSION 0.1.1 DESCRIPTION "C++ implementation of NEWUOA algorithm with C interface")

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

set(CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(newuoa
	${PROJECT_SOURCE_DIR}/src/newuoa.cpp
	${PROJECT_SOURCE_DIR}/include/newuoa.h)

add_library(newuoa::newuoa ALIAS newuoa)

set_target_properties(newuoa PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_include_directories(newuoa
	PUBLIC
	"$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>"
	"$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>"
)

# adding header sources just helps IDEs
target_sources(newuoa INTERFACE
	$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>$<INSTALL_INTERFACE:include>/newuoa.h
)

set_target_properties(newuoa PROPERTIES PUBLIC_HEADER include/newuoa.h)

# Install rules
install(TARGETS newuoa
	EXPORT newuoaTargets
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(EXPORT newuoaTargets
	FILE "newuoaTargets.cmake"
	NAMESPACE newuoa::
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/newuoa
)

install(
	DIRECTORY include
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	COMPONENT Devel
)

set(INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR} )
set(LIBRARY_INSTALL_DIR ${CMAKE_INSTALL_LIBDIR} )

configure_package_config_file(${PROJECT_SOURCE_DIR}/cmake/Config.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/newuoa/newuoaConfig.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/newuoa
	PATH_VARS INCLUDE_INSTALL_DIR LIBRARY_INSTALL_DIR
)

install(FILES
	"${CMAKE_CURRENT_BINARY_DIR}/newuoa/newuoaConfig.cmake"
	"${CMAKE_CURRENT_BINARY_DIR}/newuoa/newuoaConfigVersion.cmake"
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/newuoa
	COMPONENT Devel
)

set(newuoa_MAJOR_VERSION 0)
set_target_properties(newuoa PROPERTIES
	VERSION ${PROJECT_VERSION}
	SOVERSION 0.1
	INTERFACE_newuoa_MAJOR_VERSION 0)

set_property(TARGET newuoa APPEND PROPERTY
	COMPATIBLE_INTERFACE_STRING newuoa_MAJOR_VERSION
)

write_basic_package_version_file(
	"${CMAKE_CURRENT_BINARY_DIR}/newuoa/newuoaConfigVersion.cmake"
	VERSION ${PROJECT_VERSION}
	COMPATIBILITY AnyNewerVersion
)

# pkgconfig support

configure_file(${PROJECT_SOURCE_DIR}/cmake/newuoa.pc.in
	${CMAKE_CURRENT_BINARY_DIR}/newuoa.pc.in @ONLY)
file(GENERATE OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/newuoa.pc
	INPUT ${CMAKE_CURRENT_BINARY_DIR}/newuoa.pc.in)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/newuoa.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
