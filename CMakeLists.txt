cmake_minimum_required(VERSION 3.0)
project(DHT VERSION 0.26 LANGUAGES C)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(dht STATIC
    dht.c
    dht.h)

target_include_directories(dht
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

write_basic_package_version_file(
    "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion)

if(NOT CMAKE_SOURCE_DIR STREQUAL PROJECT_SOURCE_DIR)
    add_library(${PROJECT_NAME}::dht ALIAS dht)

    file(WRITE "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake" "")
    set(${PROJECT_NAME}_DIR "${PROJECT_BINARY_DIR}" CACHE PATH
        "The directory containing a CMake configuration file for ${PROJECT_NAME}.")
    return()
endif()

configure_file(
    Config.cmake
    ${PROJECT_NAME}Config.cmake
    @ONLY)

install(
    TARGETS dht
    EXPORT ${PROJECT_NAME}Targets)

install(
    FILES dht.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(
    FILES
        CHANGES
        dht-example.c
        LICENCE
        README
    DESTINATION ${CMAKE_INSTALL_DOCDIR})

install(
    EXPORT ${PROJECT_NAME}Targets
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
    NAMESPACE ${PROJECT_NAME}::)

install(
    FILES
        "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
        "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})
