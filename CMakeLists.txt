cmake_minimum_required(VERSION 3.28)

project(parson C)

include(GNUInstallDirs)

set(PARSON_VERSION 1.5.3)

# Create the parson library from parson.c
add_library(parson parson.c)

# Export the include directories:
# - While building: use the source root where parson.h lives
# - After install: headers will be placed under ${CMAKE_INSTALL_INCLUDEDIR}
target_include_directories(parson PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# Set public header property (used for installation)
set_target_properties(parson PROPERTIES PUBLIC_HEADER "parson.h")

# Set version properties
set_target_properties(parson PROPERTIES
    VERSION ${PARSON_VERSION}
    SOVERSION ${PARSON_VERSION}
)

# Install the parson library and its headers
install(
    TARGETS parson
    EXPORT parsonTargets
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR} COMPONENT runtime
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT runtime
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT development
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# Install CMake config file for find_package(parson)
install(
    EXPORT parsonTargets
    FILE parsonConfig.cmake
    NAMESPACE parson::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)
