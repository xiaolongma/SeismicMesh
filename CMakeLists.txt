cmake_minimum_required(VERSION 2.8.12)

project(SeismicMesh)

##############################################################################

# Add custom CMake modules
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

# Some screen output
message(STATUS "OS detected: ${CMAKE_SYSTEM_NAME}")
message(STATUS "CXX Compiler detected: ${CMAKE_CXX_COMPILER_ID}")
message(STATUS "CMake additional search path for libraries: ${CMAKE_LIBRARY_PATH}")

# CGAL and its components
find_package(CGAL)
message(STATUS "CGAL version: ${CGAL_VERSION}")

if (NOT CGAL_FOUND)
  message(STATUS "This project requires the CGAL library, and will not be compiled.")
  return()
endif()


# include helper file
include( ${CGAL_USE_FILE})

set (MIGRATION_SRCE "SeismicMesh/migration/cpp")
set (GENERATION_SRCE "SeismicMesh/generation/cpp")
set (SIZING_SRCE "SeismicMesh/sizing/cpp")

include_directories (${MIGRATION_SRCE} ${GENERATION_SRCE} ${SIZING_SRC})

add_subdirectory(pybind11)
pybind11_add_module(cpputils ${SOURCES}  "${MIGRATION_SRCE}/cpputils.cpp")
pybind11_add_module(FastHJ ${SOURCES} "${SIZING_SRCE}/FastHJ.cpp")
pybind11_add_module(c_cgal ${SOURCES} "${GENERATION_SRCE}/delaunay.cpp")