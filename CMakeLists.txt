cmake_minimum_required(VERSION 3.0)
project(sylar)

include (cmake/utils.cmake)

set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -rdynamic -O0 -ggdb -std=c++11 -Wall -Wno-deprecated -Werror -Wno-unused-function  -Wno-builtin-macro-redefined -Wno-deprecated-declarationas")

include_directories(.)

find_package(Boost REQUIRED)
if(Boost_FOUND)
        include_directories(${Boost_INCLUDE_DIRS})
endif()

find_library(YAMLCPP yaml-cpp)

set(LIB_SRC 
    sylar/log.cc
    sylar/util.cc
    sylar/thread.cc
    sylar/config.cc
    sylar/mutex.cc
    )


add_library(sylar SHARED ${LIB_SRC})
force_redefine_file_macro_for_sources(sylar)

set(LIBS  sylar pthread ${YAMLCPP})


sylar_add_executable(test "tests/test.cc" sylar "${LIBS}")
sylar_add_executable(test_config "tests/test_config.cc" sylar "${LIBS}")

sylar_add_executable(test_thread "tests/test_thread.cc" sylar "${LIBS}")
sylar_add_executable(test_util "tests/test_util.cc" sylar "${LIBS}")




set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
