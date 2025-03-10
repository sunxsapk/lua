cmake_minimum_required(VERSION 3.10)
project(lua LANGUAGES C)

# Lua version
set(LUA_VERSION 5.4)
set(LUA_RELEASE "${LUA_VERSION}.7")

# Installation paths (default to /usr/local but can be overridden)
set(INSTALL_TOP "/usr/local" CACHE PATH "Base installation directory")
set(INSTALL_BIN "${INSTALL_TOP}/bin")
set(INSTALL_INC "${INSTALL_TOP}/include")
set(INSTALL_LIB "${INSTALL_TOP}/lib")
set(INSTALL_MAN "${INSTALL_TOP}/man/man1")
set(INSTALL_LMOD "${INSTALL_TOP}/share/lua/${LUA_VERSION}")
set(INSTALL_CMOD "${INSTALL_TOP}/lib/lua/${LUA_VERSION}")

# Set the source directory
set(LUA_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

# Collect all Lua source files
file(GLOB LUA_SOURCES
    "${LUA_SRC_DIR}/*.c"
)

# Remove interpreter and compiler (we only want the library)
list(FILTER LUA_SOURCES EXCLUDE REGEX ".*(lua.c|luac.c)")

# Create the static library
add_library(${PROJECT_NAME} STATIC ${LUA_SOURCES})

# Include directory for headers
target_include_directories(${PROJECT_NAME} PUBLIC ${LUA_SRC_DIR})

# Set output name
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "lua")

# Installation rules
install(TARGETS ${PROJECT_NAME}
    ARCHIVE DESTINATION ${INSTALL_LIB}
    LIBRARY DESTINATION ${INSTALL_LIB}
)

install(FILES
    ${LUA_SRC_DIR}/lua.h
    ${LUA_SRC_DIR}/luaconf.h
    ${LUA_SRC_DIR}/lualib.h
    ${LUA_SRC_DIR}/lauxlib.h
    ${LUA_SRC_DIR}/lua.hpp
    DESTINATION ${INSTALL_INC}
)

install(PROGRAMS
    ${LUA_SRC_DIR}/lua
    ${LUA_SRC_DIR}/luac
    DESTINATION ${INSTALL_BIN}
)

install(FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/doc/lua.1
    ${CMAKE_CURRENT_SOURCE_DIR}/doc/luac.1
    DESTINATION ${INSTALL_MAN}
)

install(FILES
    "${CMAKE_BINARY_DIR}/LuaConfig.cmake"
    "${CMAKE_BINARY_DIR}/LuaConfigVersion.cmake"
    DESTINATION ${INSTALL_LIB}/cmake/Lua
)

