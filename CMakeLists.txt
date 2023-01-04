cmake_minimum_required(VERSION 3.0.0)
project(c_cxx_mixed VERSION 0.1.0 LANGUAGES C CXX)

add_executable(c_cxx_mixed src/main.c src/cpp.cpp src/cpp.hpp)
target_include_directories(c_cxx_mixed PUBLIC src/)

