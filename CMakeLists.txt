cmake_minimum_required(VERSION 3.17.0)

project(myrpc VERSION 0.1)

# Configure compiler
set(DIR_INCLUDE "include")
set(DIR_INCLUDE_MYSYSLOG "mysyslog/include")
set(DIR_SOURCE "src")

# Server variables
set(SERVER_SRC
    ${DIR_SOURCE}/list.c
    ${DIR_SOURCE}/sockutils.c
    ${DIR_SOURCE}/server/server.c
    ${DIR_SOURCE}/server/client.c
    ${DIR_SOURCE}/myrpc/myrpc_server.c
    ${DIR_SOURCE}/myrpc/protocol.c
    ${DIR_SOURCE}/myrpc/myrpc_proto.c
)

set(CLIENT_SRC
    ${DIR_SOURCE}/sockutils.c
    ${DIR_SOURCE}/client/client.c
    ${DIR_SOURCE}/myrpc/protocol.c
    ${DIR_SOURCE}/myrpc/myrpc_proto.c
)

add_subdirectory(mysyslog)

add_library(${PROJECT_NAME}_server_objs STATIC ${SERVER_SRC})
target_include_directories(${PROJECT_NAME}_server_objs PUBLIC ${DIR_INCLUDE})
target_include_directories(${PROJECT_NAME}_server_objs PUBLIC ${DIR_INCLUDE_MYSYSLOG})
target_link_libraries(${PROJECT_NAME}_server_objs mysyslog)

add_executable(${PROJECT_NAME}-server ${DIR_SOURCE}/main_server.c)

target_include_directories(${PROJECT_NAME}-server PUBLIC ${DIR_INCLUDE})
target_link_libraries(${PROJECT_NAME}-server ${PROJECT_NAME}_server_objs)

add_library(${PROJECT_NAME}_client_objs STATIC ${CLIENT_SRC})
target_include_directories(${PROJECT_NAME}_client_objs PUBLIC ${DIR_INCLUDE})
add_executable(${PROJECT_NAME}-client ${DIR_SOURCE}/main_client.c)

target_include_directories(${PROJECT_NAME}-client PUBLIC ${DIR_INCLUDE})
target_link_libraries(${PROJECT_NAME}-client ${PROJECT_NAME}_client_objs)

