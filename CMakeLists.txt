
# Set the minimum version of CMake required
cmake_minimum_required(VERSION 3.23)

# Define the project
project(
	KylbyEngine
	VERSION 1.0
	DESCRIPTION "Barebones of a wannabe engine"
	LANGUAGES CXX
)

# Set the C++ standard to C++20
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(
	CMAKE_RUNTIME_OUTPUT_DIRECTORY 
	"${CMAKE_BINARY_DIR}/$<CONFIGURATION>/out"
)
set(
	CMAKE_LIBRARY_OUTPUT_DIRECTORY 
	"${CMAKE_BINARY_DIR}/$<CONFIGURATION>/lib"
)

add_executable(KylbyEngine 
	src/main.cpp
)

# Specify where the program have to look for runtime libraries
if(APPLE)
	set_target_properties(KylbyEngine PROPERTIES 
		INSTALL_RPATH "@executable_path;@loader_path"
		BUILD_WITH_INSTALL_RPATH TRUE
		MACOSX_RPATH TRUE
	)
elseif(UNIX)
	set_target_properties(KylbyEngine PROPERTIES
		INSTALL_RPATH "$ORIGIN;$ORIGIN/../lib"
		BUILD_WITH_INSTALL_RPATH TRUE
	)
endif()

if(WIND32)
	set(MATH_LIBRARY "" CACHE STRING "" FORCE)
endif()

# Specify that we want to build SDL_TTF's dependencies
set(SDLTTF_VENDORED ON)

# Set a variable for our include directory
set(SDL_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/include/SDL_libs")

# Add the SDL libraries from our submodules
add_subdirectory(${SDL_INCLUDE_DIR}/SDL)
add_subdirectory(${SDL_INCLUDE_DIR}/SDL_image)
add_subdirectory(${SDL_INCLUDE_DIR}/SDL_ttf)

target_link_libraries(KylbyEngine 
	SDL3::SDL3 
	SDL3_image::SDL3_image 
	SDL3_ttf::SDL3_ttf
)
