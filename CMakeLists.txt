cmake_minimum_required(VERSION 3.20)
project(application_name VERSION 0.0.1)

# Compiler detection
set(COMPILER Unknown)
if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
  set(COMPILER Clang)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  set(COMPILER GCC)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "Intel")
  set(COMPILER Intel)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  set(COMPILER MSVC)
endif()
message("-- Compiler detected: " ${COMPILER})

set(CMAKE_CXX_STANDARD 17)

# List of all source files to compile and link
set(SOURCES src/main.cpp)

# List of folders contatinig header files
set(INCLUDE_DIRS include)

# Include all the folders contatining header files
include_directories(${INCLUDE_DIRS})

add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})
