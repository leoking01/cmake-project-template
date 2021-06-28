cmake_minimum_required(VERSION 3.15)
project(division   VERSION 0.1.0 LANGUAGES CXX  )

set(SOURCE_FILES
    division.h
    division.cpp
    )

#add_library(division SHARED STATIC ${SOURCE_FILES})
add_library(division SHARED   ${SOURCE_FILES})

install(TARGETS division DESTINATION ${DIVISIBLE_INSTALL_LIB_DIR})
install(FILES division.h DESTINATION ${DIVISIBLE_INSTALL_INCLUDE_DIR})













#-- ------install  destation --- --------
#set( DESTINATION  "../install/")
set( CMAKE_INSTALL_PREFIX  "../install/")
#          And to the end of the top-level CMakeLists.txt we add:
install(TARGETS ${PROJECT_NAME} DESTINATION bin)
#install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"  DESTINATION include  )









#
# Format the project using the `clang-format` target (i.e: cmake --build build --target clang-format)
#

#add_clang_format_target()

#
# Install library for easy downstream inclusion
#

include(GNUInstallDirs)
install(
    TARGETS
    ${PROJECT_NAME}
    EXPORT
    ${PROJECT_NAME}Targets
    LIBRARY DESTINATION
    ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION
    ${CMAKE_INSTALL_BINDIR}
    ARCHIVE DESTINATION
    ${CMAKE_INSTALL_LIBDIR}
    INCLUDES DESTINATION
    include
    PUBLIC_HEADER DESTINATION
    include
    )

install(
    EXPORT
    ${PROJECT_NAME}Targets
    FILE
    ${PROJECT_NAME}Targets.cmake
    NAMESPACE
    ${PROJECT_NAME}::
    DESTINATION
    ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
    )

#
# Add version header
#

configure_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/version.hpp.in
    include/${PROJECT_NAME_LOWERCASE}/version.hpp
    @ONLY
    )

install(
    FILES
    ${CMAKE_CURRENT_BINARY_DIR}/include/${PROJECT_NAME_LOWERCASE}/version.hpp
    DESTINATION
    include/${PROJECT_NAME_LOWERCASE}
    )

#
# Install the `include` directory
#

install(
    DIRECTORY
    include/${PROJECT_NAME_LOWERCASE}
    DESTINATION
    include
    )

#verbose_message("Install targets succesfully build. Install with `cmake --build <build_directory> --target install --config <build_config>`.")

#
# Quick `ConfigVersion.cmake` creation
#

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${PROJECT_NAME}ConfigVersion.cmake
    VERSION
    ${PROJECT_VERSION}
    COMPATIBILITY
    SameMajorVersion
    )

configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION
    ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
    )

install(
    FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    DESTINATION
    ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
    )

#
# Generate export header if specified
#

if(${PROJECT_NAME}_GENERATE_EXPORT_HEADER)
    include(GenerateExportHeader)
    generate_export_header(${PROJECT_NAME})
    install(
        FILES
        ${PROJECT_BINARY_DIR}/${PROJECT_NAME_LOWERCASE}_export.h
        DESTINATION
        include
        )

    message(STATUS "Generated the export header `${PROJECT_NAME_LOWERCASE}_export.h` and installed it.")
endif()

message(STATUS "Finished building requirements for installing the package.\n")

#
# Unit testing setup
#
if(  0 )
    if(${PROJECT_NAME}_ENABLE_UNIT_TESTING)
        enable_testing()
        message(STATUS "Build unit tests for the project. Tests should always be found in the test folder\n")
        add_subdirectory(test)
    endif()
endif()











