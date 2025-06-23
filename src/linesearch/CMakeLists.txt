cmake_minimum_required(VERSION 3.21)
list(APPEND CMAKE_MESSAGE_CONTEXT cls)
project(linesearch VERSION 0.1.0)
set(CMAKE_C_STANDARD 11)

add_library(linesearch_linesearch
  linesearch.cpp
  linesearch.hpp
#  linesearch.h
#  linesearch.c
  cubicspline.c
  cubicspline.h
)
target_include_directories(linesearch_linesearch
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
)
add_library(linesearch::linesearch ALIAS linesearch_linesearch)
