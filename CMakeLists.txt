cmake_minimum_required(VERSION 3.25)
project(PJSIP)

option(LINUX_POLL OFF)

enable_testing()

add_subdirectory(pjlib)
add_subdirectory(third_party)
add_subdirectory(pjlib-util)
add_subdirectory(pjnath)
add_subdirectory(pjmedia)
add_subdirectory(pjsip)