cmake_minimum_required(VERSION 3.29)

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
    
target_include_directories(raknet PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src")

install(TARGETS raknet RUNTIME DESTINATION "${CMAKE_SOURCE_DIR}/bin"
			LIBRARY DESTINATION "${CMAKE_SOURCE_DIR}/lib"
			ARCHIVE DESTINATION "${CMAKE_SOURCE_DIR}/lib")
