cmake_minimum_required (VERSION 2.8.8)
project (Web-Server)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Wextra")
include_directories(.)
find_package(Threads REQUIRED)
set(BOOST_COMPONENTS system thread filesystem date_time)
if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.9)
        set(BOOST_COMPONENTS ${BOOST_COMPONENTS} regex)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DUSE_BOOST_REGEX")
    endif()
endif()
find_package(Boost 1.53.0 COMPONENTS ${BOOST_COMPONENTS} REQUIRED)
include_directories(SYSTEM ${Boost_INCLUDE_DIR})

add_executable(web_server web_server.cpp)
target_link_libraries(web_server ${Boost_LIBRARIES})
target_link_libraries(web_server ${CMAKE_THREAD_LIBS_INIT})

enable_testing()

install(FILES server_http.hpp DESTINATION include/web-server)
