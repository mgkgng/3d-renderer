cmake_minimum_required(VERSION 3.10)
project(SCOP)

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Add subdirectories
add_subdirectory(external/glfw)
add_subdirectory(external/glad)

# Include directories
include_directories(
    external/glfw/include
    external/glad/include
    external/stb_image
    include
    src
)

# Source files
file(GLOB_RECURSE SOURCES
    src/*.cpp
    external/glad/src/glad.c
    external/stb_image/stb_image.cpp
)

# Executable
add_executable(SCOP ${SOURCES})

# Link libraries
target_link_libraries(SCOP glfw ${CMAKE_DL_LIBS})

# macOS-specific flags (OpenGL is in framework)
if(APPLE)
    target_link_libraries(SCOP "-framework OpenGL")
else()
    find_package(OpenGL REQUIRED)
    target_link_libraries(SCOP OpenGL::GL)
endif()
