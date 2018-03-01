

# Version requirement and project info
cmake_minimum_required(VERSION 3.8)
project(remotery)

# Build options
set(ALLOW_OPENGL 	OFF 					CACHE BOOL 		"Allow OpenGL profiling")
set(WEBSITE_PREFIX  "${CMAKE_BINARY_DIR}" 	CACHE STRING 	"Prefix folder for the website files")

# Source files
file(GLOB SOURCE_FILES lib/*.c)

# Define target as static library
add_library(remotery ${SOURCE_FILES})

# Link to libut
target_link_libraries(remotery)

# Add top include directory as public dependency.
# This automatically adds them to parent projects when linking to libcl.
target_include_directories(remotery PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/lib)

# Set macros depending on build options
if(ALLOW_OPENGL)
	target_compile_definitions(remotery PUBLIC -DRMT_USE_OPENGL=1)
endif()

# Set user variables
set(LIBREMOTERY_LIBRARIES remotery CACHE INTERNAL "")

# Copy web site files to build directory
add_custom_command(
	TARGET remotery
	POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_directory "${CMAKE_CURRENT_SOURCE_DIR}/vis" "${WEBSITE_PREFIX}/vis"
)
