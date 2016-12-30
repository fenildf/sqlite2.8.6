cmake_minimum_required(VERSION 3.5)
project(sqlite2)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -DSQLITE_CORE=1")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}) #like gcc: -I
file(GLOB sources "*.c")
file(GLOB headers "*.h")
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/lempar.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/lemon.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/test1.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/test2.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/test3.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/threadtest.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/md5.c)
list(REMOVE_ITEM sources ${CMAKE_CURRENT_SOURCE_DIR}/tclsqlite.c)

add_executable(sqlite2 ${sources} ${headers})