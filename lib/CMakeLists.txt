# Copyright 2010-2013,2018-2019 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

find_package(Threads)

add_library(pmt
  SHARED
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_unv.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_io.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_pool.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_serialize.cc
)

target_link_libraries(pmt ${CMAKE_THREADS_LIBS_INIT})

target_include_directories(pmt
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../include
)

