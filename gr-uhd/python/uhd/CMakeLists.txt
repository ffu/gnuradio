# Copyright 2011-212 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# Setup python install
########################################################################
include(GrPython)

gr_python_install(FILES __init__.py DESTINATION ${GR_PYTHON_DIR}/gnuradio/uhd)

if(PYQT5_FOUND)
    gr_python_install(FILES replaymsgpushbutton.py DESTINATION ${GR_PYTHON_DIR}/gnuradio/uhd)
endif(PYQT5_FOUND)

if(ENABLE_UHD_RFNOC)
    gr_python_install(FILES grc_workflows.py DESTINATION ${GR_PYTHON_DIR}/gnuradio/uhd)
endif(ENABLE_UHD_RFNOC)

########################################################################
# Handle the unit tests
########################################################################
if(ENABLE_TESTING)

    list(APPEND GR_TEST_PYTHON_DIRS ${PROJECT_BINARY_DIR}/gr-uhd/python
         ${PROJECT_BINARY_DIR}/gnuradio-runtime/python)

    list(APPEND GR_TEST_TARGET_DEPS gnuradio-uhd)

    include(GrTest)
    file(GLOB py_qa_test_files "qa_*.py")
    foreach(py_qa_test_file ${py_qa_test_files})
        get_filename_component(py_qa_test_name ${py_qa_test_file} NAME_WE)
        if(CMAKE_CROSSCOMPILING)
            get_filename_component(py_qa_test_file ${py_qa_test_file} NAME)
        endif(CMAKE_CROSSCOMPILING)
        gr_add_test(${py_qa_test_name} ${QA_PYTHON_EXECUTABLE} -B ${py_qa_test_file})
    endforeach(py_qa_test_file)
endif(ENABLE_TESTING)

add_subdirectory(bindings)
