cmake_minimum_required(VERSION 3.20)
project(application_name)

set(CMAKE_CXX_STANDARD 17)

# List of all source files to compile and link
set(SOURCES src/main.cpp)

# List of folders contatinig header files
set(INCLUDE_DIRS include)

# Include all the folders contatining header files
include_directories(${INCLUDE_DIRS})

add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})
