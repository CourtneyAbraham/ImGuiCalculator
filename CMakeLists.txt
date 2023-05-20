cmake_minimum_required(VERSION 3.16)

project(CppProjectTemplate VERSION 1.0.0 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD          17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS        OFF)

option(ENABLE_WARNINGS "Enable compiler warnings for a target" ON)
option(ENABLE_WARNINGS_AS_ERRORS "Enable compiler warnings as errors" OFF)

option(ENABLE_SANITIZE_ADDR "Enable address sanitizer" ON)
option(ENABLE_SANITIZE_UNDEF "Enable undefined sanitizer" ON)

option(ENABLE_LTO "Enable LTO/IPO" ON)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake/")
include(AddGitSubmodule)

if(ENABLE_WARNINGS)
    include(Warnings)
endif()

if(ENABLE_SANITIZE_ADDR OR ENABLE_SANITIZE_UNDEF)
    include(Sanitizer)
    add_sanitizer_flags()
endif()

if(ENABLE_LTO)
    include(LTO)
endif()

add_git_submodule(vendor/glfw)
add_git_submodule(vendor/imgui)

add_executable(ImGuiCalculator src/main.cpp)

target_include_directories(ImGuiCalculator PUBLIC "./src/")
target_link_libraries(ImGuiCalculator glfw imgui)
target_set_warnings(
        ImGuiCalculator
        ON
        OFF
)
target_enable_lto(ImGuiCalculator)