cmake_minimum_required(VERSION 3.5)
project(EVIO VERSION 4.4.6 LANGUAGES C CXX)

set(CMAKE_MODULE_PATH
  ${CMAKE_MODULE_PATH}
  ${PROJECT_SOURCE_DIR}/cmake
  ${PROJECT_SOURCE_DIR}/cmake/modules)

### Use C++17 features 
#set(CMAKE_CXX_STANDARD 17)
#set(CMAKE_CXX_STANDARD_REQUIRED ON)

## OS Specific settings
include(cmake/os.cmake)

include(GNUInstallDirs)
set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/evio)

find_package(EXPAT REQUIRED COMPONENTS EXPAT )
find_package(Threads REQUIRED)
find_package(ZLIB REQUIRED)

add_subdirectory(src/libsrc)
add_subdirectory(src/libsrc++)
add_subdirectory(src/execsrc)
#src/examples
#src/test

##############################################
# Installation instructions

#Create a ConfigVersion.cmake file
include(CMakePackageConfigHelpers)

# Install and export targets
install(EXPORT evio-targets
  FILE EVIOTargets.cmake
  NAMESPACE EVIO::
  DESTINATION lib/evio
  )

set(TARGETS_INSTALL_PATH lib/evio/EVIOTargets.cmake)
configure_package_config_file(${CMAKE_CURRENT_LIST_DIR}/cmake/EVIOConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/EVIOConfig.cmake
    INSTALL_DESTINATION lib/evio
    PATH_VARS TARGETS_INSTALL_PATH
)

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/EVIOConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
  )


#Install the config, configversion and custom find modules
install(FILES
  #    ${CMAKE_CURRENT_LIST_DIR}/cmake/FindRapidJSON.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/EVIOConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/EVIOConfigVersion.cmake
  DESTINATION lib/evio
)

##############################################
## Exporting from the build tree
#configure_file(${CMAKE_CURRENT_LIST_DIR}/cmake/FindRapidJSON.cmake
#    ${CMAKE_CURRENT_BINARY_DIR}/FindRapidJSON.cmake
#    COPYONLY)

export(EXPORT evio-targets FILE ${CMAKE_CURRENT_BINARY_DIR}/EVIOTargets.cmake NAMESPACE EVIO::)

#Register package in user's package registry
export(PACKAGE EVIO)

