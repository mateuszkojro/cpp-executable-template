cmake_minimum_required(VERSION 3.20)
project(application_name VERSION 0.0.1)

message("")
message("=== CONFIGURING ENVIROMENT ${CMAKE_PROJECT_NAME} - START  ===")
message("")

# Operation system detection
message(CHECK_START "Checking operating system")
if(WIN32)
  set(OS Windows)
  add_compile_definitions(MK_OS_WINDOWS)
  message(CHECK_PASS "Windows detected")
elseif(UNIX)
  set(OS Unix)
  add_compile_definitions(MK_OS_UNIX)
  message(CHECK_PASS "Unix detected")
else()
  message(CHECK_FAIL "Unknown operating system")
endif()

# Build type detection
message(CHECK_START "Checking build type")
if(CMAKE_BUILD_TYPE STREQUAL Release OR CMAKE_BUILD_TYPE STREQUAL RelWithDebInfo)
  set(RELEASE)
  add_compile_definitions(MK_BUILD_RELEASE)
  message(CHECK_PASS "Release")
elseif(CMAKE_BUILD_TYPE STREQUAL Debug)
  set(RELEASE)
  add_compile_definitions(MK_BUILD_DEBUG)
  message(CHECK_PASS "Debug")
elseif(CMAKE_BUILD_TYPE STREQUAL "")
  set(DEBUG)
  add_compile_definitions(MK_BUILD_DEBUG)
  message(CHECK_PASS "Unknown build type defaulting to debug")
else()
set(DEBUG)
  add_compile_definitions(MK_BUILD_DEBUG)
  message(CHECK_FAIL "Unsuported build type: \"${CMAKE_BUILD_TYPE}\" defaulting to debug")
endif()

# Compiler detection
message(CHECK_START "Checking compiler")
if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
  set(COMPILER Clang)
  message(CHECK_PASS "Clang compiler detected")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  set(COMPILER GCC)
  message(CHECK_PASS "GNU compiler detected")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "Intel")
  set(COMPILER Intel)
  message(CHECK_PASS "Intel compiler detected")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  set(COMPILER MSVC)
  message(CHECK_PASS "MSVC detected")
else()
  message(CHECK_FAIL "Unknown compiler")
endif()

set(CMAKE_CXX_STANDARD 14)

# Check what libraries are available

set(LIBRARIES_TO_LINK)

message(CHECK_START "Looking for Eigen")
set(EIGEN_DIR ${CMAKE_SOURCE_DIR}/lib/eigen/)
if(EXISTS ${EIGEN_DIR})
    list(APPEND CMAKE_MESSAGE_INDENT "    ")
    include_directories(${EIGEN_DIR})
    add_subdirectory(${EIGEN_DIR})
    add_compile_definitions(MK_WITH_EIGEN)
    list(POP_BACK CMAKE_MESSAGE_INDENT)
    message(CHECK_PASS "found")
else()
    message(CHECK_FAIL "not found")
endif()

message(CHECK_START "Looking for ROOT")
find_package(ROOT CONFIG)
if(${ROOT_FOUND})
    include(${ROOT_USE_FILE})
    include_directories(${ROOT_INCLUDE_DIRS})
    list(APPEND LIBRARIES_TO_LINK ROOT::Core ROOT::Gui)
    add_compile_definitions(MK_WITH_ROOT)
    message(CHECK_PASS "found")
else()
    message(CHECK_FAIL "not found")
endif()


message(CHECK_START "Looking for Sciplot")
set(SCIPLOT_DIR ${CMAKE_SOURCE_DIR}/lib/sciplot/)
if(EXISTS ${SCIPLOT_DIR})
    include_directories(${SCIPLOT_DIR})
    add_compile_definitions(MK_WITH_PLOT)
    message(CHECK_PASS "found")
else()
    message(CHECK_FAIL "not found")
endif()

# List of all source files to compile and link
set(SOURCES src/main.cpp)

# List of folders contatinig header files
set(INCLUDE_DIRS include)

# Include all the folders contatining header files
include_directories(${INCLUDE_DIRS})

add_executable(${CMAKE_PROJECT_NAME} ${SOURCES})

message("-- Linking ${CMAKE_PROJECT_NAME} against: ${LIBRARIES_TO_LINK}")
target_link_libraries(${CMAKE_PROJECT_NAME} PUBLIC ${LIBRARIES_TO_LINK})