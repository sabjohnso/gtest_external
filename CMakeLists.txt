include(ExternalProject)
include(GNUInstallDirs)

set( GTEST_PREFIX ${CMAKE_CURRENT_BINARY_DIR}/gtest)
set( GTEST_LIB_DIR ${GTEST_PREFIX}/${CMAKE_INSTALL_LIBDIR} )
set( GTEST_INCLUDE_DIR ${GTEST_PREFIX}/${CMAKE_INSTALL_INCDIR} )

set( GTEST_LIB ${GTEST_LIB_DIR}/libgtest.a )
set( GTEST_MAIN_LIB ${GTEST_LIB_DIR}/libgtest_main.a )
set( GMOCK_LIB ${GTEST_LIB_DIR}/libgmock.a )
set( GMOCK_MAIN_LIB ${GTEST_LIB_DIR}/libgmock_main.a )

externalproject_add( googletest_external
  GIT_REPOSITORY      https://github.com/google/googletest.git
  PREFIX              ${GTEST_PREFIX}
  SOURCE_DIR          ${GTEST_PREFIX}/src
  CMAKE_ARGS          -DCMAKE_INSTALL_PREFIX=${GTEST_PREFIX}
  BUILD_BYPRODUCTS
    ${GTEST_LIB}
    ${GTET_MAIN_LIB}
    ${GMOCK_LIB}
    ${GMOCK_MAIN_LIB}
  UPDATE_DISCONNECTED ON)

add_library( googletest INTERFACE )
target_include_directories( googletest INTERFACE ${GTEST_INCLUDE_DIR})
target_link_libraries( googletest INTERFACE ${GTEST_LIB} pthread )
add_dependencies( googletest googletest_external)


add_library( googletest_main INTERFACE )
target_include_directories( googletest_main INTERFACE
  INTERFACE $<BUILD_INTERFACE:${GTEST_INCLUDE_DIR}>)
target_link_libraries( googletest_main INTERFACE  ${GTEST_MAIN_LIB} googletest PRIVATE pthread )
add_dependencies( googletest_main googletest_external)


add_library( googlemock INTERFACE )
target_include_directories( googlemock INTERFACE
  INTERFACE $<BUILD_INTERFACE:${GTEST_INCLUDE_DIR}> )
target_link_libraries( googlemock INTERFACE ${GMOCK_LIB} googletest PRIVATE pthread )
add_dependencies( googlemock googletest_external)


add_library( googlemock_main INTERFACE )
target_include_directories( googlemock_main INTERFACE
  INTERFACE $<BUILD_INTERFACE:${GEST_INCLUDE_DIR}>)
target_link_libraries( googlemock_main INTERFACE ${GMOCK_MAIN_LIB} googlemock PRIVATE pthread )
add_dependencies( googlemock_main googletest_external)


add_library(google::test ALIAS googletest)
add_library(google::test_main ALIAS googletest_main)
add_library(google::mock ALIAS googlemock)
add_library(google::mock_main ALIAS googlemock_main)
