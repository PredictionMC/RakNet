cmake_minimum_required(VERSION 3.15)

project(RakNet)

file(GLOB_RECURSE RAKNET_SOURCES "src/*.cpp")

add_library(raknet "${RAKNET_SOURCES}")

target_compile_features(raknet PUBLIC cxx_std_20)

if(MSVC)
    target_compile_definitions(raknet PRIVATE -D_CRT_SECURE_NO_WARNINGS)
endif()

IF (WIN32 AND NOT UNIX)
    target_link_libraries(raknet ws2_32)
ENDIF (WIN32 AND NOT UNIX)

IF(BUILD_SHARED_LIBS)
    target_compile_definitions(raknet PUBLIC _RAKNET_DLL)
ENDIF (BUILD_SHARED_LIBS)

target_include_directories(raknet
    PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>"
        "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>")

target_include_directories(raknet PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/include/RakNet")

install(TARGETS raknet 
    EXPORT raknetTargets
    RUNTIME DESTINATION "${CMAKE_SOURCE_DIR}/bin"
	LIBRARY DESTINATION "${CMAKE_SOURCE_DIR}/lib"
	ARCHIVE DESTINATION "${CMAKE_SOURCE_DIR}/lib")
