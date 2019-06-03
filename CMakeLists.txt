#Intebded to be use with Deestream docker image
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
set(CMAKE_CXX_STANDARD 14)
project(ds-myapp)

enable_language(C)

if (DEFINED ENV{DS_SDK_ROOT})
    set(DS_SDK_ROOT $ENV{DS_SDK_ROOT})
else ()
    set(DS_SDK_ROOT "/root/DeepStream_Release/")
endif ()

message(STATUS "System name is ${CMAKE_SYSTEM_NAME}")

STRING(TOLOWER "${CMAKE_SYSTEM_NAME}" CMAKE_SYSTEM_NAME_LOWER_CASE)
STRING(TOLOWER "${CMAKE_BUILD_TYPE}" CMAKE_BUILD_TYPE_LOWER)

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
endif()

find_package(PkgConfig REQUIRED)
pkg_check_modules(GST REQUIRED gstreamer-1.0>=1.4
                               gstreamer-video-1.0>=1.4
                               gstreamer-app-1.0>=1.4)

include_directories(${DS_SDK_ROOT}/sources/includes)

link_directories(${GST_LIBRARY_DIRS} /usr/local/deepstream)

include_directories(${GST_INCLUDE_DIRS} ${DS_SDK_ROOT}/sources/includes)
add_executable(my-app my-app.c)
target_link_libraries(my-app ${GST_LIBRARIES} rt m nvdsgst_helper nvdsgst_meta)
