cmake_minimum_required(VERSION 3.0)

project(net)

# Set build type to RELEASE by default:
if (NOT CMAKE_BUILD_TYPE)
  message(STATUS "No build type provided, default to RELEASE.")
  set(CMAKE_BUILD_TYPE "RELEASE")
endif ()
 
option(BuildShared "BuildShared" ON)
option(Sanitize "Sanitize" OFF)

if (BuildShared)
  set(LIB_TYPE SHARED)
else ()
  set(LIB_TYPE STATIC)
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
include_directories(. include )

set(CMAKE_CXX_STANDARD 11) 
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "-Wall -std=c++11 ${CMAKE_CXX_FLAGS}")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
set(CMAKE_CXX_FLAGS_DEBUG "-g -pedantic -fno-omit-frame-pointer -fno-inline-functions")

if (Sanitize)
  set(CMAKE_CXX_FLAGS "-fsanitize=address -fsanitize=undefined ${CMAKE_CXX_FLAGS}")
endif ()

add_subdirectory(src)
add_subdirectory(logic-gates)
add_subdirectory(swig)

message(STATUS "")
message(STATUS "  BUILD SUMMARY")
message(STATUS "  CMAKE_GENERATOR      : ${CMAKE_GENERATOR}")
message(STATUS "  Compiler ID          : ${CMAKE_CXX_COMPILER_ID}")
message(STATUS "  Build type           : ${CMAKE_BUILD_TYPE}")
message(STATUS "  Library type         : ${LIB_TYPE}")
message(STATUS "  CXX_FLAGS            : ${CMAKE_CXX_FLAGS}")
if ("${CMAKE_BUILD_TYPE}" STREQUAL "RELEASE")
  message(STATUS "  CXX_FLAGS_RELEASE    : ${CMAKE_CXX_FLAGS_RELEASE}")
elseif ("${CMAKE_BUILD_TYPE}" STREQUAL "DEBUG")
  message(STATUS "  CXX_FLAGS_DEBUG      : ${CMAKE_CXX_FLAGS_DEBUG}")
endif ()
message(STATUS "")
