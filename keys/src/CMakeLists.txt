cmake_minimum_required(VERSION 3.1)

set(prj_base keys)

set(src
    key.cpp
    keyslib.cpp
    pubkey.cpp
    bitcoinaddress.cpp
    )

add_library(${prj_base}_dynamic SHARED ${src})
add_library(${prj_base}_static STATIC ${src})

target_link_libraries(${prj_base}_dynamic ${script_dynamic})
target_link_libraries(${prj_base}_static ${script_static})

target_include_directories(${prj_base}_dynamic 
    PRIVATE 
    ../include/${prj_base}
    ${CMAKE_SOURCE_DIR}/script/include
    ${CMAKE_SOURCE_DIR}/structs/include
    ${CMAKE_SOURCE_DIR}/utils/include
    ${CMAKE_SOURCE_DIR}/crypto/include
    )

target_include_directories(${prj_base}_static 
    PRIVATE 
    ../include/${prj_base}
    ${CMAKE_SOURCE_DIR}/script/include
    ${CMAKE_SOURCE_DIR}/structs/include
    ${CMAKE_SOURCE_DIR}/utils/include
    ${CMAKE_SOURCE_DIR}/crypto/include
    )

set_target_properties(${prj_base}_dynamic PROPERTIES OUTPUT_NAME "hdac_${prj_base}")
set_target_properties(${prj_base}_static PROPERTIES VERSION 1.2 SOVERSION 1)

set_target_properties(${prj_base}_static PROPERTIES OUTPUT_NAME "hdac_${prj_base}")

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

install(TARGETS ${prj_base}_dynamic ${prj_base}_static
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

file(GLOB headers ../include/${prj_base}/*.h )
install(FILES ${headers} DESTINATION include/hdac/${prj_base})