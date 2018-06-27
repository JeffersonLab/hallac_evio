cmake_minimum_required(VERSION 3.5)

add_library(evio SHARED
  evio.c
  eviofmt.c
  eviofmtswap.c
  evioswap.c
  )
add_library(evioStatic STATIC 
  evio.c
  eviofmt.c
  eviofmtswap.c
  evioswap.c
  )

set(headers 
  evio.h
  msinttypes.h
  )

#Add an alias so that library can be used inside the build tree, e.g. when testing
add_library(EVIO::EVIO ALIAS evio)
add_library(EVIO::EVIOStatic ALIAS evioStatic)

#Set target properties
target_include_directories(evio
    PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)
target_include_directories(evioStatic
    PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

#target_compile_features(jsonutils PRIVATE cxx_auto_type)
#target_compile_options(jsonutils PRIVATE $<$<CXX_COMPILER_ID:GNU>:-Wall>)

target_link_libraries(evio
  PUBLIC ${EXPAT_LIBRARIES}
  PUBLIC ${CMAKE_THREAD_LIBS_INIT}
  PUBLIC ${ZLIB_LIBRARIES}
)

target_link_libraries(evioStatic
  PUBLIC ${EXPAT_LIBRARIES}
  PUBLIC ${CMAKE_THREAD_LIBS_INIT}
  PUBLIC ${ZLIB_LIBRARIES}
)

#set_target_properties(evioStatic PROPERTIES OUTPUT_NAME evioStatic)

install(TARGETS evio evioStatic
    EXPORT evio-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
set_target_properties(evio PROPERTIES EXPORT_NAME EVIO)
set_target_properties(evioStatic PROPERTIES EXPORT_NAME EVIOStatic)

#This is required so that the exported target has the name JSONUtils and not jsonutils

install(FILES ${headers} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

#Export the targets to a script
install(EXPORT evio-targets
  FILE
  EVIOTargets.cmake
  NAMESPACE
  EVIO::
  DESTINATION
    ${INSTALL_CONFIGDIR}
)
