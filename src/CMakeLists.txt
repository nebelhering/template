cmake_minimum_required(VERSION 3.10)

add_library(template template.cpp)

target_include_directories(template PUBLIC .)

target_link_libraries(template PRIVATE warning)
