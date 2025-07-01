# Used 3.23 features
# - PROJECT_IS_TOP_LEVEL (3.21)
# - FILE_SETS (3.23)
cmake_minimum_required(VERSION 3.23)
list(APPEND CMAKE_MESSAGE_CONTEXT altro)
project(altro VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)

# Organization info
set(ORGANIZATION_NAME_FULL "Robotic Exploration Lab")
set(ORGANIZATION_NAME "rexlab")

# Include needed modules
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
include(CMakePrintHelpers)
include(FetchContent)
include(FindThreads)
include(GNUInstallDirs)

#############################################
# Options
#############################################

# Handle default build type
set(ALTRO_DEFAULT_BUILD_TYPE "Release")
if (NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "No build type specified. Setting CMAKE_BUILD_TYPE to ${ALTRO_DEFAULT_BUILD_TYPE}")
  set(CMAKE_BUILD_TYPE ${ALTRO_DEFAULT_BUILD_TYPE} CACHE STRING "Choose the type of build." FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# Code Coverage
option(ALTRO_CODECOV "Compile altro with code coverage" OFF)
if (ALTRO_CODE_COVERAGE AND CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
  message(STATUS "Compiler altro with coverage info.")
  add_compile_options(-O0 -g -fprofile-arcs -ftest-coverage)
endif()

# Documentation

# Use -fPIC option
#option(ALTRO_POSITION_INDEPENDENT_CODE "Use -fPIC flag when compiling altro" ON)
if (BUILD_SHARED_LIBS)
#  set(CMAKE_POSITION_INDEPENDENT_CODE ON FORCE)
  message(STATUS "Building shared library.")
  add_compile_options(-fPIC)
else()
  message(STATUS "Building static library. Note that a shared library is recommended.")
endif()

# Set default install prefix to /opt/rexlab/altro
if(NOT WIN32 AND CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT AND PROJECT_IS_TOP_LEVEL)
  message(STATUS "Setting Altro install prefix to /opt/${ORGANIZATION_NAME}/${PROJECT_NAME}")
  set(CMAKE_INSTALL_PREFIX "/opt/${ORGANIZATION_NAME}/${PROJECT_NAME}" CACHE FILEPATH "Base install location" FORCE)
  set(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT OFF)
endif()

#############################################
# Dependencies
#############################################
add_subdirectory(deps)

#############################################
# Documentation
#############################################

#############################################
# Build
#############################################

# Output all binaries
add_compile_options(-march=native)
add_subdirectory(src)

#############################################
# Testing
#############################################

#############################################
# Installation
#############################################
add_subdirectory(install)
