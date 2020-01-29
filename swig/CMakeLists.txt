# Copyright 2013,2019 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# Setup swig generation
########################################################################
include(GrPython)
include(GrSwig)

###############################################################
#    Build pmt swig
###############################################################

set(GR_SWIG_DOC_FILE ${CMAKE_CURRENT_BINARY_DIR}/pmt_swig_doc.i)
set(GR_SWIG_DOC_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/../include/pmt
  ${CMAKE_CURRENT_BINARY_DIR}/../include/pmt)
GR_SWIG_MAKE(pmt_swig pmt_swig.i)

GR_SWIG_INSTALL(
    TARGETS pmt_swig
    DESTINATION ${GR_PYTHON_DIR}/pmt
)

install(
    FILES
    pmt_swig.i
    DESTINATION ${GR_INCLUDE_DIR}/gnuradio/swig
)
