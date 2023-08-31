cmake_minimum_required(VERSION 3.15)

set(nonsense_VERSION 1.0.1)

project(nonsense
	VERSION ${nonsense_VERSION}
	DESCRIPTION "A nonsense library containing some stubs to make up a useless library to link to"
	HOMEPAGE_URL "https://github.com/ftbmynameis/nonsense"
	LANGUAGES CXX)
	
include(CMakePackageConfigHelpers)
include(GNUInstallDirs)
	
add_library(nonsense src/nonsense.cpp include/nonsense.h)
add_library(nonsense::nonsense ALIAS nonsense)

if(BUILD_SHARED_LIBS AND WIN32)
	target_compile_definitions(nonsense
		PRIVATE "NONSENSE_EXPORT=__declspec(dllexport)"
		INTERFACE "NONSENSE_EXPORT=__declspec(dllimport)")
endif()

target_compile_features(nonsense PUBLIC cxx_std_17)
target_include_directories(nonsense
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
		$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)


configure_package_config_file(cmake/config.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/nonsense-config.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_DATADIR}/nonsense
	NO_SET_AND_CHECK_MACRO)
write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/nonsense-config-version.cmake
	VERSION ${nonsense_VERSION}
	COMPATIBILITY SameMajorVersion)
install(
	FILES
		${CMAKE_CURRENT_BINARY_DIR}/nonsense-config.cmake
		${CMAKE_CURRENT_BINARY_DIR}/nonsense-config-version.cmake
	DESTINATION
		${CMAKE_INSTALL_DATADIR}/nonsense)

install(TARGETS nonsense EXPORT nonsense-targets)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(EXPORT nonsense-targets
	NAMESPACE nonsense::
	DESTINATION ${CMAKE_INSTALL_DATADIR}/nonsense)