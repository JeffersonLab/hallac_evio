cmake_minimum_required(VERSION 3.5)


add_library(evioxx SHARED
evioBankIndex.cc
evioBufferChannel.cc
evioDictionary.cc
evioException.cc
evioFileChannel.cc
evioSocketChannel.cc
evioUtil.cc
)

add_library(EVIO::EVIOcpp ALIAS evioxx)

#Set target properties
target_include_directories(evioxx
    PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

#target_compile_features(jsonutils PRIVATE cxx_auto_type)
#target_compile_options(jsonutils PRIVATE $<$<CXX_COMPILER_ID:GNU>:-Wall>)

target_link_libraries(evioxx
  PUBLIC ${EXPAT_LIBRARIES}
  PUBLIC ${CMAKE_THREAD_LIBS_INIT}
  PRIVATE EVIO::EVIO
)
#include(GNUInstallDirs)
#set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/JSONUtils)

install(TARGETS evioxx
    EXPORT evio-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

#This is required so that the exported target has the name JSONUtils and not jsonutils
#set_target_properties(EVIOcpp PROPERTIES EXPORT_NAME EVIOcpp)


set(headers
evioBankIndex.hxx
evioBufferChannel.hxx
evioChannel.hxx
evioDictionary.hxx
evioException.hxx
evioFileChannel.hxx
evioSocketChannel.hxx
evioTypedefs.hxx
evioUtil.hxx
evioUtilTemplates.hxx
)

install(FILES ${headers} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/evio)

#Export the targets to a script
install(EXPORT evio-targets
  FILE
  EVIOTargets.cmake
  NAMESPACE
  EVIO::
  DESTINATION
    ${INSTALL_CONFIGDIR}
)
