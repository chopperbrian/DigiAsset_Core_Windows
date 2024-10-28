cmake_minimum_required(VERSION 3.16)
project(digiasset_core-cli)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
set(CMAKE_CXX_STANDARD 20)
# FIND_PACKAGE(JSONCPP REQUIRED)
# FIND_PACKAGE(JSONRPCCPP REQUIRED)
# find_package(CURL REQUIRED)

# add_library(llibcurl STATIC IMPORTED)


include_directories("E:\\vcpkg\\vcpkg-master\\installed\\x64-windows\\include")
include_directories("E:\\digi asset original\\DigiAsset_Core\\src")
include_directories("C:\\Program Files (x86)\\libjson-rpc-cpp 0.4.1\\include")



add_compile_options(/EHsc)


set(HEADER_FILES
        ../src/Config.h
        ../src/DigiByteCore.cpp
        )

set(SOURCE_FILES
        ../src/Config.cpp
        ../src/DigiByteCore.cpp
        )

add_executable(digiasset_core-cli main.cpp ${SOURCE_FILES} ${HEADER_FILES})
# 
link_directories("E:\\libjson-rpc-cpp\\build\\lib\\Debug")
link_directories("E:\\vcpkg\\vcpkg-master\\installed\\x64-windows\\lib")

target_link_libraries(${PROJECT_NAME} jsoncpp.lib sqlite3.lib libssl.lib libcrypto.lib jsonrpccpp-common.lib jsonrpccpp-server.lib jsonrpccpp-client.lib libcurl.lib  )


# target_link_libraries(digiasset_core-cli PRIVATE CURL::libcurl)
# target_link_libraries(digiasset_core-cli PRIVATE ssl)
# target_link_libraries(digiasset_core-cli PRIVATE pthread)

# target_link_libraries(digiasset_core-cli PRIVATE /lib/x86_64-linux-gnu/libjsoncpp.so)
# target_link_libraries(digiasset_core-cli PRIVATE jsonrpccpp-common)
# target_link_libraries(digiasset_core-cli PRIVATE jsonrpccpp-client)


install(TARGETS digiasset_core-cli DESTINATION bin)