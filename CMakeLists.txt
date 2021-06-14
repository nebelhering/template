cmake_minimum_required(VERSION 3.10)
project(Template)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_EXTENSIONS false)
set(CMAKE_EXPORT_COMPILE_COMMANDS true)


find_program(CCACHE_FOUND ccache)
if (CCACHE_FOUND)
    message("-- Detecting ccache")
    set_property(GLOBAL PROPERTY RULE_LAUNCH_COMPILE ccache)
    set_property(GLOBAL PROPERTY RULE_LAUNCH_LINK ccache)
    message("-- Dececting ccache - done")
endif (CCACHE_FOUND)

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    add_compile_options (-fdiagnostics-color=always)
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    add_compile_options (-fcolor-diagnostics)
endif ()

add_compile_options(-march=native)

find_package(Boost REQUIRED)
find_package(fmt REQUIRED)
find_package(doctest REQUIRED)

add_library(warnings INTERFACE)
target_compile_options(warnings INTERFACE
        -pedantic
        -Wall
        -Wextra
        -Wshadow
        -Wnon-virtual-dtor
        -Wold-style-cast
        -Wcast-align
        -Wunused
        -Woverloaded-virtual
        -Wpedantic
        -Wconversion
        -Wsign-conversion
        -Wmisleading-indentation
        -Wduplicated-cond
        -Wduplicated-branches
        -Wlogical-op
        -Wnull-dereference
        -Wuseless-cast
        -Wdouble-promotion
        -Wformat=2)

add_executable(Template main.cpp)

add_subdirectory(src)

set(Template_LIBS
    template
    warnings
    fmt::fmt
    pthread)

target_link_libraries(Template PRIVATE ${Template_LIBS})

