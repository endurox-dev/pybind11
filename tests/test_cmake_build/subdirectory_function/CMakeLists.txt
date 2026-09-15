cmake_minimum_required(VERSION 3.4...3.18)

project(test_subdirectory_function CXX)

add_subdirectory("${pybind11_SOURCE_DIR}" pybind11)
pybind11_add_module(test_subdirectory_function ../main.cpp)
set_target_properties(test_subdirectory_function PROPERTIES OUTPUT_NAME test_cmake_build)

if(DEFINED Python_EXECUTABLE)
  set(_Python_EXECUTABLE "${Python_EXECUTABLE}")
elseif(DEFINED PYTHON_EXECUTABLE)
  set(_Python_EXECUTABLE "${PYTHON_EXECUTABLE}")
else()
  message(FATAL_ERROR "No Python executable defined (should not be possible at this stage)")
endif()

add_custom_target(
  check_subdirectory_function
  ${CMAKE_COMMAND}
  -E
  env
  PYTHONPATH=$<TARGET_FILE_DIR:test_subdirectory_function>
  ${_Python_EXECUTABLE}
  ${PROJECT_SOURCE_DIR}/../test.py
  ${PROJECT_NAME}
  DEPENDS test_subdirectory_function)
