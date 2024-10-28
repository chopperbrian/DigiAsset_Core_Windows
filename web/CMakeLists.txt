cmake_minimum_required(VERSION 3.16)
project(digiasset_core-web)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
set(CMAKE_CXX_STANDARD 17)


set(HEADER_FILES
        ../src/Config.h
        )

set(SOURCE_FILES
        ../src/Config.cpp
        )

add_compile_options(/EHsc)


include_directories("E:\\vcpkg\\vcpkg-master\\installed\\x64-windows\\include")
include_directories("E:\\digi asset original\\DigiAsset_Core\\src")
include_directories("C:\\Program Files (x86)\\libjson-rpc-cpp 0.4.1\\include")
include_directories("E:\\vcpkg\\vcpkg-master\\vcpkg2\\vcpkg\\installed\\x64-windows\\include")



add_executable(digiasset_core-web main.cpp ${SOURCE_FILES} ${HEADER_FILES})

target_link_libraries(${PROJECT_NAME} jsoncpp.lib sqlite3.lib libssl.lib libcrypto.lib jsonrpccpp-common.lib jsonrpccpp-server.lib jsonrpccpp-client.lib libcurl.lib  )

install(TARGETS digiasset_core-web DESTINATION bin)