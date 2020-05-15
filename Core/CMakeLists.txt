add_library(Library.Core STATIC stub.cpp)
target_include_directories(Library.Core PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_compile_features(Library.Core PUBLIC cxx_std_17)

include(FetchContent)
FetchContent_Declare(zlib
		URL https://github.com/EQMG/zlib/archive/v1.2.11.1.tar.gz
		)
FetchContent_GetProperties(zlib)
if(NOT zlib_POPULATED)
	set(BUILD_TESTING OFF CACHE INTERNAL "")
	set(SKIP_INSTALL_LIBRARIES ON CACHE INTERNAL "")
	FetchContent_Populate(zlib)
	add_subdirectory(${zlib_SOURCE_DIR} ${zlib_BINARY_DIR})
	set_target_properties(zlib PROPERTIES FOLDER External)
endif()
target_link_libraries(Library.Core PUBLIC zlib)
# Other third_party applications using `find_package(zlib)` will link to our zlib build.
# https://github.com/Kitware/CMake/blob/master/Modules/FindZLIB.cmake
set(ZLIB_INCLUDE_DIRS "${zlib_SOURCE_DIR}" CACHE STRING "" FORCE)
set(ZLIB_LIBRARIES zlib CACHE STRING "" FORCE)
set(ZLIB_FOUND TRUE CACHE STRING "" FORCE)
