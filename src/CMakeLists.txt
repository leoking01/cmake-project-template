cmake_minimum_required(VERSION 3.15)
project(divider      VERSION 0.1.0 LANGUAGES CXX  )

#add_subdirectory(division)
set(SOURCE_FILES main.cpp)
include_directories(  ../division )

add_executable(divider ${SOURCE_FILES})
target_link_libraries(divider division)
install(TARGETS divider DESTINATION ${DIVISIBLE_INSTALL_BIN_DIR})




















