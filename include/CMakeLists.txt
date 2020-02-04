# Copyright 2010-2013 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# Install the headers
########################################################################
install(FILES
  pmt/api.h
  pmt/pmt.h
  pmt/pmt_pool.h
  pmt/pmt_sugar.h
  pmt/pmt_serial_tags.h
  DESTINATION ${CMAKE_INSTALL_PREFIX}/include/pmt
)

install(FILES
    attributes.h
    DESTINATION ${CMAKE_INSTALL_PREFIX}/include/
)
