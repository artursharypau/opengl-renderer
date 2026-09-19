cmake_minimum_required(VERSION 3.10)
project(opengl_renderer LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(PROJECT_SOURCE_FILES
    src/main.cpp
)

include_directories(src)
add_executable(${PROJECT_NAME} ${PROJECT_SOURCE_FILES})

# Add GLFW library
add_subdirectory(thirdparty/glfw-3.4 "${CMAKE_CURRENT_BINARY_DIR}/glfw_build")
include_directories(thirdparty/glfw-3.4/include)

# Add Glew library
set(BUILD_UTILS OFF CACHE BOOL "utilities" FORCE)
add_subdirectory(thirdparty/glew/build/cmake "${CMAKE_CURRENT_BINARY_DIR}/glew_build")
include_directories(thirdparty/glew/include)
set_property(TARGET glew_s PROPERTY FOLDER GLEW)
set_property(TARGET glew   PROPERTY FOLDER GLEW)

# Link all thirdparty libraries
target_link_libraries(${PROJECT_NAME}
    glfw
    glew_s
)

# macOS: OpenGL is deprecated since 10.14, silence the warnings
if (APPLE)
    target_compile_definitions(${PROJECT_NAME} PRIVATE GL_SILENCE_DEPRECATION)
endif()
