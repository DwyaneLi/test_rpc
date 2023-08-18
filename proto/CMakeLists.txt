# 查找指定文件夹中的文件
file(GLOB FOLDER_FILES "${CMAKE_SOURCE_DIR}/pb")

# 检查文件夹是否存在
if(NOT FOLDER_FILES)
    execute_process(
        COMMAND mkdir ${CMAKE_SOURCE_DIR}/pb
        RESULT_VARIABLE result1)
endif()

message("Generating echo pb files...")

set(PROTOC_COMMAND "/home/io500/rdma-based-rpc/bin/protoc")

set(PROTOC_ARGS "--cpp_out=${CMAKE_SOURCE_DIR}/pb" "hello.proto")

execute_process(
    COMMAND ${PROTOC_COMMAND} ${PROTOC_ARGS}
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    RESULT_VARIABLE result2
)

if(result2 EQUAL 0)
    message("protobuf 编译成功！")
else()
    message("protobuf 编译失败！")
endif()
