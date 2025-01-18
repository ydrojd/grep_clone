cmake_minimum_required(VERSION 3.20)

project(grep_clone)

set(CMAKE_EXPORT_COMPILE_COMMANDS true)

set (target grep_clone)

add_executable(${target})

target_compile_features(${target} PRIVATE cxx_std_23)

set_target_properties(${target} PROPERTIES CXX_EXTENSIONS OFF)

target_sources(${target}
    PRIVATE
    src/grep.cpp)

target_include_directories(${target} PRIVATE include/)

target_compile_options(${target} PRIVATE
    # warnings
    -Wall -Wextra -Wpedantic -Wshadow -Wnon-virtual-dtor
    # Release
    $<$<CONFIG:Release>:-O2>
    # Debug
    $<$<CONFIG:Debug>:-Og -g -fno-omit-frame-pointer>
    # DebugSanitize
    $<$<CONFIG:DebugSanitize>:-Og -g -fno-omit-frame-pointer -fsanitize=undefined -fsanitize-recover=undefined -fsanitize=address -fsanitize-recover=address>)

target_link_options(${target} PRIVATE $<$<CONFIG:DebugSanatize>:-fsanitize=undefined -fsanitize=address>)
