cmake_minimum_required(VERSION 3.14)
project(opengltriangle)

set(CMAKE_CXX_STANDARD 17)

set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(LIB_DIR "${CMAKE_CURRENT_SOURCE_DIR}/lib")

add_executable(${PROJECT_NAME} src/main.cpp src/Shader.cpp src/Shader.h)

set(GLAD_DIR "${LIB_DIR}/glad")
add_library("glad" "${GLAD_DIR}/src/glad.c")
target_include_directories("glad" PRIVATE "${GLAD_DIR}/include")
target_include_directories(${PROJECT_NAME} PRIVATE "${GLAD_DIR}/include")
target_link_libraries(${PROJECT_NAME} "glad" "${CMAKE_DL_LIBS}")

set(GLFW_DIR "${LIB_DIR}/glfw")
set(GLFW_BUILD_EXAMPLES OFF CACHE INTERNAL "Build the GLFW example programs")
set(GLFW_BUILD_TESTS OFF CACHE INTERNAL "Build the GLFW test programs")
set(GLFW_BUILD_DOCS OFF CACHE INTERNAL "Build the GLFW documentation")
set(GLFW_INSTALL OFF CACHE INTERNAL "Generate installation target")
add_subdirectory("${GLFW_DIR}")
target_compile_definitions(${PROJECT_NAME} PRIVATE "GLFW_INCLUDE_NONE")
target_include_directories(${PROJECT_NAME} PRIVATE "${GLFW_DIR}/include")
target_link_libraries(${PROJECT_NAME} "glfw" "${GLFW_LIBRARIES}")

if (APPLE)
    message(STATUS "Platform is Apple")
    add_compile_definitions(PLATFORM_APPLE)
endif ()

if (WIN32)
    message(STATUS "Platform is Windows")
    add_compile_definitions(PLATFORM_WINDOWS)
endif ()
