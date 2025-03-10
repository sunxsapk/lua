cmake_minimum_required(VERSION 3.15)

project(lua)

# include(ExternalProject)
# 
# ExternalProject_Add(lua
#    URL "https://www.lua.org/ftp/lua-5.3.5.tar.gz"
#    CONFIGURE_COMMAND ""
#    BUILD_COMMAND make generic
#    BUILD_ALWAYS true
#    BUILD_IN_SOURCE true
#    INSTALL_COMMAND ""
# )
# add_library(liblua STATIC IMPORTED)
# ExternalProject_Get_property(lua SOURCE_DIR)
# message("liblua will be found at \'${SOURCE_DIR}/src\'")


project(lua VERSION 5.4 LANGUAGES CXX)

file(GLOB_RECURSE LUA_SRC_FILES
    "src/*.c"
)

# Optionally, set the target name for the static library
add_library(${PROJECT_NAME} STATIC ${LUA_SRC_FILES})

# Optionally, set properties for the static library (like the output directory)
# set_target_properties(${PROJECT_NAME} PROPERTIES
#     ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/libs"
# )

# Add any required include directories for Lua headers
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Define a custom target to run the existing Makefile (in case there are additional build steps in the Makefile)
add_custom_target(lua_build
    COMMAND make
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

