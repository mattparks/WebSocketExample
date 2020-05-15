add_library(Library.Encrypt STATIC stub.cpp)
target_include_directories(Library.Encrypt PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_compile_features(Library.Encrypt PUBLIC cxx_std_17)
target_link_libraries(Library.Encrypt PUBLIC Library.Core)

include(FetchContent)
FetchContent_Declare(openssl
		URL https://github.com/spauldingmedical/openssl/archive/1_1_1d-r2.tar.gz
		)
FetchContent_GetProperties(openssl)
if(NOT openssl_POPULATED)
	set(BUILD_SHARED_OPENSSL ON CACHE INTERNAL "Build shared openssl lib")
	FetchContent_Populate(openssl)
	add_subdirectory(${openssl_SOURCE_DIR} ${openssl_BINARY_DIR})
	set_target_properties(crypto PROPERTIES FOLDER External)
	set_target_properties(ssl PROPERTIES FOLDER External)
endif()
target_link_libraries(Library.Encrypt PRIVATE crypto ssl)
# Other third_party applications using `find_package(OpenSSL)` will link to our OpenSSL build.
# https://github.com/Kitware/CMake/blob/master/Modules/FindOpenSSL.cmake
set(OPENSSL_INCLUDE_DIR "${openssl_SOURCE_DIR}/include" CACHE STRING "" FORCE)
set(OPENSSL_LIBRARIES crypto ssl CACHE STRING "" FORCE)
set(OPENSSL_FOUND TRUE CACHE STRING "" FORCE)
