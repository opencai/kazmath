MESSAGE("Building lkazmath wrapper")

FILE(GLOB_RECURSE KAZMATH_C_FILES ${CMAKE_SOURCE_DIR}/kazmath/*.c ${CMAKE_CURRENT_SOURCE_DIR}/*.c)

INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/kazmath)

FIND_PACKAGE(Lua51)

IF(LUA51_FOUND)
    SET(CMAKE_POSITION_INDEPENDENT_CODE ON)

    LIST(REMOVE_ITEM KAZMATH_C_FILES ${CMAKE_SOURCE_DIR}/kazmath/GL/mat4stack.c)
    LIST(REMOVE_ITEM KAZMATH_C_FILES ${CMAKE_SOURCE_DIR}/kazmath/GL/matrix.c)

    ADD_LIBRARY(lkazmath SHARED ${KAZMATH_C_FILES})
    TARGET_LINK_LIBRARIES(lkazmath ${LUA_LIBRARIES})
    include_directories(${LUA_INCLUDE_DIR})

    INSTALL(TARGETS lkazmath LIBRARY DESTINATION ${INSTALL_LIB_DIR})
ELSE()
    MESSAGE("Unable to build Lua bindings as lua was not installed on the system")
ENDIF()
