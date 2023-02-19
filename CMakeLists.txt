cmake_minimum_required(VERSION 3.25.0)
project(whkdb VERSION 0.1.0)

include(CTest)
enable_testing()

# ImGui
execute_process(
    COMMAND git submodule update --init --recursive
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

set(imgui_SOURCE_DIR ./imgui)

add_library(imgui STATIC
    ${imgui_SOURCE_DIR}/imgui.h
    ${imgui_SOURCE_DIR}/imgui_internal.h
    ${imgui_SOURCE_DIR}/imgui.cpp
    ${imgui_SOURCE_DIR}/imgui_demo.cpp
    ${imgui_SOURCE_DIR}/imgui_draw.cpp
    ${imgui_SOURCE_DIR}/imgui_tables.cpp
    ${imgui_SOURCE_DIR}/imgui_widgets.cpp
    ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
    ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp)

target_include_directories(imgui PUBLIC
    ${imgui_SOURCE_DIR}/
    ${imgui_SOURCE_DIR}/backends)

# GLFW & OpenGL
find_package(glfw3 REQUIRED)
find_package(OpenGL REQUIRED)

add_executable(whkdb main.cpp)

target_link_libraries(whkdb imgui)
target_link_libraries(whkdb glfw)
target_link_libraries(whkdb OpenGL)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

