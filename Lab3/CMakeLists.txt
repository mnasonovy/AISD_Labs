cmake_minimum_required(VERSION 3.11)

project(sorting CXX)

if(MSVC)
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS true)
    if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
        string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    else()
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
    endif()
endif()

include(FetchContent)
FetchContent_Declare(googletest URL https://github.com/google/googletest/archive/6b63c98ac43efc992122f1da12aaf0a0e0658206.zip)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

add_library(sorting
    "include/stats.h"
     "include/sorting_functions.h"
      "include/task_functions.h")

set_target_properties(sorting PROPERTIES LINKER_LANGUAGE CXX)
target_compile_features(sorting PUBLIC cxx_std_23)

add_executable(sorting_test
    "src/sorting.cc")
target_link_libraries(sorting_test gtest gtest_main sorting)
target_include_directories(sorting_test PRIVATE "${CMAKE_SOURCE_DIR}")

add_executable(task_test
    "src/task.cc")
target_link_libraries(task_test gtest gtest_main sorting)
target_include_directories(task_test PRIVATE "${CMAKE_SOURCE_DIR}")