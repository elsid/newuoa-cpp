cmake_minimum_required (VERSION 3.9)
project (newuoa VERSION 0.1.0 DESCRIPTION "libnewuoa")
include(GNUInstallDirs)
add_library(newuoa SHARED src/newuoa.cpp)
set_target_properties(newuoa PROPERTIES
	VERSION ${PROJECT_VERSION}
	SOVERSION 0
	PUBLIC_HEADER include/newuoa.h)
configure_file(newuoa.pc.in newuoa.pc @ONLY)
target_include_directories(newuoa PRIVATE include)
install(TARGETS newuoa
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(FILES ${CMAKE_BINARY_DIR}/newuoa.pc
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
