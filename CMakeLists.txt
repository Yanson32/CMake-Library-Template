#for backwards compatibility
cmake_minimum_required(VERSION 3.5.1)

#set output directory for binaries
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")

option(BUILD_EXAMPLES "Set to TRUE if you want to build examples" FALSE)
option(BUILD_DOC "Set to TRUE if you want to build library documentation" FALSE)
option(BUILD_TESTING "Set to TRUE if you want to build unit tests" FALSE)

set(VALID_BUILD_TYPES "Release" "Debug" "MinSizeRel" "RelWithDebInfo")

project("CMakLibraryTemplate" VERSION "0.0.0")

#set default build type
if(NOT CMAKE_CONFIGURATION_TYPES)
	if(CMAKE_BUILD_TYPE STREQUAL "")
		set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Select build type [${VALID_BUILD_TYPES}]" FORCE)
	endif()
endif()

#set dropdown list in cmake gui
if(NOT CMAKE_CONFIGURATION_TYPES)
	set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS ${VALID_BUILD_TYPES})
endif()

#Make sure the build type is valid
if(NOT CMAKE_CONFIGURATION_TYPES)
	list(FIND GU_VALID_BUILD_TYPES "${CMAKE_BUILD_TYPE}" INDEX)
	if(${INDEX} MATCHES -1)
		message(FATAL_ERROR "Invalid build type <${CMAKE_BUILD_TYPE}>. Valid types are [${VALID_BUILD_TYPES}]")
	endif()
endif()

# uninstall target
if(NOT TARGET uninstall)
  configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

  add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
endif()

include("CPackConfig")
include("CPack")
