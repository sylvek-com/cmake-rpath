cmake_minimum_required(VERSION 3.0)
project("cml" LANGUAGES CXX)
if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(FATAL_ERROR "In-source builds disabled")
endif()
# diversion: how to recognize non-default python
#
set(Python_ADDITIONAL_VERSIONS 3)
include(FindPythonInterp)
include(FindPythonLibs)
message("Python_NAMES=" ${_Python_NAMES})
message("Python_VERSIONS=" ${_Python_VERSIONS})
#
# end diversion
#
set(CMAKE_BUILD_TYPE "Release" CACHE STRING "None Debug Release RelWithDebInfo MinSizeRel" FORCE)
set(CMAKE_VERBOSE_MAKEFILE FALSE CACHE BOOL ".SILENT override" FORCE)
set(CMAKE_INSTALL_PREFIX "$ENV{HOME}/local" CACHE PATH "prefix override" FORCE)
#
# undocumented file() command arguments
# RPATH_CHECK,RPATH_CHANGE,RPATH_REMOVE
#
add_library(l SHARED l.cpp)
set(CMAKE_SKIP_RPATH ON)
set(CMAKE_SKIP_BUILD_RPATH ON)
set(CMAKE_BUILD_RPATH "$ORIGIN")
set(CMAKE_INSTALL_RPATH ":")
set(CMAKE_BUILD_WITH_INSTALL_RPATH ON)
add_executable(m m.cpp)
target_link_libraries(m l)
install(TARGETS m l DESTINATION .)
