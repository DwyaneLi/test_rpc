cmake_minimum_required(VERSION 3.0)
project(client)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Threads REQUIRED)

add_subdirectory(proto)

set(INCLUDE_DIR_1 "${CMAKE_SOURCE_DIR}/pb")
# 设置库文件和头文件的路径
set(CLIENT_LIB_DIR "/home/io500/rdma-based-rpc/install/lib/x86_64-linux-gnu")  # libclient.so 所在的目录
set(CLIENT_INCLUDE_DIR 
    "/home/io500/rdma-based-rpc/install/include/rdma_rpc")  # client.hh 所在的目录

include_directories(
    ${INCLUDE_DIR_1}
    ${CLIENT_INCLUDE_DIR}
)

# 添加可执行文件
add_executable(client)

target_sources(
  client
  PRIVATE 
  client.cpp
  ./pb/hello.pb.cc
  PUBLIC 
  ./pb/hello.pb.h
)

 
# 添加库目录和头文件目录
target_link_directories(client PRIVATE ${CLIENT_LIB_DIR})

# 添加需要链接的外部库
target_link_libraries(client
    PUBLIC
    rdma_rpc
    protobuf
    protobuf-lite
    Threads::Threads
)

add_executable(server)

target_sources(
    server
    PRIVATE
    server.cpp
    ./pb/hello.pb.cc
    PUBLIC
    ./pb/hello.pb.h
)

# 添加库目录和头文件目录
target_link_directories(server PRIVATE ${CLIENT_LIB_DIR})

# 添加需要链接的外部库
target_link_libraries(server
    PUBLIC
    rdma_rpc
    protobuf
    protobuf-lite
    Threads::Threads
)

install(TARGETS client RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(TARGETS server RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})


