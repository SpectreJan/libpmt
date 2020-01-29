# Copyright 2010-2013,2018-2019 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

add_library(gnuradio-pmt
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_unv.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_io.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_pool.cc
  ${CMAKE_CURRENT_SOURCE_DIR}/pmt_serialize.cc
)

target_link_libraries(gnuradio-pmt
  Boost::boost
  Boost::thread
  Log4Cpp::log4cpp
)

target_include_directories(gnuradio-pmt
  PUBLIC
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_PREFIX}/include>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../../include>
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/
  )


#Add Windows DLL resource file if using MSVC
if(MSVC)
    include(${CMAKE_SOURCE_DIR}/cmake/Modules/GrVersion.cmake)

    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/gnuradio-pmt.rc.in
        ${CMAKE_CURRENT_BINARY_DIR}/gnuradio-pmt.rc
    @ONLY)

    target_sources(gnuradio-pmt PRIVATE
        ${CMAKE_CURRENT_BINARY_DIR}/gnuradio-pmt.rc
    )
endif(MSVC)

if(BUILD_SHARED_LIBS)
    set_target_properties(gnuradio-pmt PROPERTIES SOVERSION "1.0")
    set_target_properties(gnuradio-pmt PROPERTIES VERSION "1.0")

    #install the generated files like so...
    install(TARGETS gnuradio-pmt
        EXPORT gnuradio-pmt-export
        LIBRARY DESTINATION lib # .so/.dylib file
        ARCHIVE DESTINATION lib # .lib file
        RUNTIME DESTINATION lib/gnuradio # .dll file
        )

    #install the exported target files
    install(EXPORT gnuradio-pmt-export
      FILE gnuradio-pmtTargets.cmake
      NAMESPACE gnuradio::
      DESTINATION ${CMAKE_MODULES_DIR}/gnuradio
      )

    include(CMakePackageConfigHelpers)
    set(TARGET gnuradio-pmt)
    set(TARGET_DEPENDENCIES ${ARGN})

    configure_package_config_file(
      ${PROJECT_SOURCE_DIR}/cmake/Modules/targetConfig.cmake.in
      ${CMAKE_CURRENT_BINARY_DIR}/cmake/Modules/gnuradio-pmtConfig.cmake
      INSTALL_DESTINATION ${CMAKE_MODULES_DIR}/gnuradio
      )
    install(
      FILES ${CMAKE_CURRENT_BINARY_DIR}/cmake/Modules/gnuradio-pmtConfig.cmake
      DESTINATION ${CMAKE_MODULES_DIR}/gnuradio
      )
endif()
