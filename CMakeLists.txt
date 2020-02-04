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

execute_process(
    COMMAND "${PYTHON_EXECUTABLE}" -c
    "from __future__ import print_function\ntry:\n import numpy\n import os\n inc_path = numpy.get_include()\n if os.path.exists(os.path.join(inc_path, 'numpy', 'arrayobject.h')):\n  print(inc_path, end='')\nexcept:\n pass"
    OUTPUT_VARIABLE PYTHON_NUMPY_INCLUDE_DIR
)

enable_testing()

########################################################################
# PyBind11 Related
########################################################################
find_package(pybind11 REQUIRED)
add_subdirectory(lib)
add_subdirectory(include)
add_subdirectory(python)
