# Tools CMake

if(DEFINED COMPOUND_PROJECT)
	
else()
	cmake_minimum_required(VERSION 3.31)
	project(rootengine_tools)
	
endif()

add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/third_party/")

add_library(data_types STATIC "${CMAKE_CURRENT_SOURCE_DIR}/data_types/data_types.c")
target_include_directories(data_types PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/data_types/include/")

add_library(data_struct STATIC "${CMAKE_CURRENT_SOURCE_DIR}/data_struct/data_struct.c")
target_include_directories(data_struct PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/data_struct/include/")
target_link_libraries(data_struct data_types)

add_library(roots_math STATIC "${CMAKE_CURRENT_SOURCE_DIR}/roots_math/roots_math.c")
target_include_directories(roots_math PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/roots_math/include/")

add_library(asset_io STATIC "${CMAKE_CURRENT_SOURCE_DIR}/asset_io/asset_io.c")
target_include_directories(asset_io PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/asset_io/include/")
target_link_libraries(asset_io stbi ufbx data_struct)

if(DEFINED COMPOUND_PROJECT)
else()
	message("Building Standalone project...")
	#file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/assets/" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}/assets/")
	add_executable(tools_standalone "${CMAKE_CURRENT_SOURCE_DIR}/tools_main.cpp")
	target_link_libraries(tools_standalone glad glfw asset_io data_struct roots_math)
endif()