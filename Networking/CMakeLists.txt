add_library(Library.Networking STATIC stub.cpp)
target_include_directories(Library.Networking PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
target_compile_features(Library.Networking PUBLIC cxx_std_17)
target_link_libraries(Library.Networking PUBLIC Library.Core Library.Encrypt)

include(FetchContent)
FetchContent_Declare(ixwebsocket
	#	URL https://github.com/machinezone/IXWebSocket/archive/v9.5.8.tar.gz
		URL https://github.com/machinezone/IXWebSocket/archive/v9.1.9.tar.gz
		)
FetchContent_GetProperties(ixwebsocket)
if(NOT ixwebsocket_POPULATED)
	set(USE_TLS ON CACHE INTERNAL "Add TLS support")
	set(USE_MBED_TLS OFF CACHE INTERNAL "Use Mbed TLS")
	set(USE_OPEN_SSL ON CACHE INTERNAL "Use OpenSSL")
	FetchContent_Populate(ixwebsocket)
	add_subdirectory(${ixwebsocket_SOURCE_DIR} ${ixwebsocket_BINARY_DIR})
	set_target_properties(ixwebsocket PROPERTIES FOLDER External)
endif()
target_link_libraries(Library.Networking PRIVATE ixwebsocket)
