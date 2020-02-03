# Copyright 2011,2013 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# GNU Radio is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3, or (at your option)
# any later version.
#
# GNU Radio is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNU Radio; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

include(GrPython)

########################################################################
# Install python files
########################################################################
GR_PYTHON_INSTALL(FILES
    __init__.py
    pmt_to_python.py
    DESTINATION ${GR_PYTHON_DIR}/pmt
)

########################################################################
# Python Bindings
########################################################################

pybind11_add_module(pmt_python ${CMAKE_CURRENT_SOURCE_DIR}/python_bindings.cpp)
target_include_directories(pmt_python PUBLIC
    ${PYTHON_NUMPY_INCLUDE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../../lib
    ${CMAKE_CURRENT_SOURCE_DIR}/../../include
    ${PYBIND11_INCLUDE_DIR} 
)
target_link_libraries(pmt_python PUBLIC ${Boost_LIBRARIES} ${PYTHON_LIBRARIES} pmt)

# if(WIN32)
#     set(PMT_PYTHON_LIBRARY_NAME libpmt_python.pyd)
# else()
#     set(PMT_PYTHON_LIBRARY_NAME libpmt_python${CMAKE_SHARED_MODULE_SUFFIX})
# endif(WIN32)

# add_custom_command(TARGET pmt_python
#     POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:pmt_python> ${CMAKE_CURRENT_BINARY_DIR}/${PMT_PYTHON_LIBRARY_NAME})

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR} DESTINATION ${GR_PYTHON_DIR} COMPONENT pythonapi)

#Debug variable names
# get_cmake_property(_variableNames VARIABLES)
# list (SORT _variableNames)
# foreach (_variableName ${_variableNames})
#     message(STATUS "${_variableName}=${${_variableName}}")
# endforeach()


########################################################################
# Setup unit tests
#######################################################################
if(UNIX)

    file(GLOB py_qa_test_files "qa_*.py")

    foreach(py_qa_test_file ${py_qa_test_files})

        get_filename_component(py_qa_test_name ${py_qa_test_file} NAME_WE)
        set(LD_PATH_VAR "LD_LIBRARY_PATH")
        if(APPLE)
            set(LD_PATH_VAR "DYLD_LIBRARY_PATH")
        endif(APPLE)

        set(binpath "${CMAKE_CURRENT_BINARY_DIR}:$PATH")
        list(APPEND pypath "${CMAKE_CURRENT_BINARY_DIR}/../:$PYTHONPATH")

        set(SHELL "/bin/sh")
        set(sh_file ${CMAKE_CURRENT_BINARY_DIR}/${py_qa_test_name}_test.sh)
        file(WRITE ${sh_file} "#! ${SHELL}\n")
        file(APPEND ${sh_file} "export PATH=${binpath}\n")
        file(APPEND ${sh_file} "export PYTHONPATH=${pypath}\n")
        file(APPEND ${sh_file} "${PYTHON_EXECUTABLE} -B ${CMAKE_CURRENT_SOURCE_DIR}/${py_qa_test_name}.py")
        #make the shell file executable
        execute_process(COMMAND chmod +x ${sh_file})

        add_test(${py_qa_test_name} ${SHELL} ${sh_file})
    endforeach(py_qa_test_file)
endif(UNIX)
