cmake_minimum_required(VERSION 3.10)

project(lua LANGUAGES C)

# Set the source directory
set(LUA_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Collect all Lua source files
file(GLOB LUA_SOURCES
    "${LUA_SRC_DIR}/*.c"
)

# Remove unwanted files (like lua.c and luac.c, which are for the Lua interpreter/compiler)
list(FILTER LUA_SOURCES EXCLUDE REGEX ".*(lua.c|luac.c)")

# Create the static library
add_library(${PROJECT_NAME} STATIC ${LUA_SOURCES})

# Include the source directory for headers
target_include_directories(${PROJECT_NAME} PUBLIC ${LUA_SRC_DIR})

# Set output properties
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "lua")

# Install the library and headers
install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${LUA_SRC_DIR}/ DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)


