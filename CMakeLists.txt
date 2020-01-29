# Copyright 2011-2012,2014,2017 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(FATAL_ERROR "Prevented in-tree build. This is bad practice.")
endif(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})

########################################################################
# Project setup
########################################################################
# Make sure this version matches ${GR_CMAKE_MIN_VERSION} (a variable can't be
# used here).
cmake_minimum_required(VERSION 3.8)
project(libpmt)

# Make sure our local CMake Modules path comes first
list(INSERT CMAKE_MODULE_PATH 0 ${CMAKE_SOURCE_DIR}/cmake/Modules)
option(BUILD_SHARED_LIBS "Build shared libraries" ON)

if(NOT CMAKE_MODULES_DIR)
  SET(CMAKE_MODULES_DIR lib${LIB_SUFFIX}/cmake)
endif(NOT CMAKE_MODULES_DIR)

find_package(LOG4CPP REQUIRED)
find_package(Boost COMPONENTS thread)
find_package(SWIG REQUIRED)
add_subdirectory(lib)
add_subdirectory(include)
add_subdirectory(python)
add_subdirectory(swig)
