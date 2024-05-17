# MARK: - 1. Preface
cmake_minimum_required(VERSION 3.28)
set(PROJECT_VERSION 2.5.1)
set(PROJECT_NAME jbigkit)
project(${PROJECT_NAME} VERSION ${PROJECT_VERSION} LANGUAGES C)
set(PACKAGE_CONFIGNAME ${PROJECT_NAME})
set(PACKAGE_VERSION ${PROJECT_VERSION})


# MARK: - 2. General
set(CMAKE_C_STANDARD 17)

include_directories(src/include)

# @TODO+:Slava -> Better explain why this is needed. For configuration files generation I suppose?
include(GNUInstallDirs)
set(INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR})
set(LIB_INSTALL_DIR ${CMAKE_INSTALL_LIBDIR})
set(BIN_INSTALL_DIR ${CMAKE_INSTALL_BINDIR})


# MARK: - 3. Libraries
add_library(libjbig SHARED src/lib/jbig.c src/lib/jbig_ar.c)
set_target_properties(libjbig PROPERTIES PUBLIC_HEADER "src/include/jbig.h;src/include/jbig_ar.h")

add_library(libjbig85 SHARED src/lib/jbig85.c src/lib/jbig_ar.c)
set_target_properties(libjbig85 PROPERTIES PUBLIC_HEADER "src/include/jbig85.h;src/include/jbig_ar.h")

set(JBIGKIT_TARGETS libjbig libjbig85)


# MARK: - 4. Apps
option(INSTALL_APPS "Build binary utilities for jbg->pbm and pbm->jpg conversion" OFF)
if(INSTALL_APPS)
    add_executable(pbmtojbg src/bin/pbmtojbg.c)
    target_include_directories(pbmtojbg PRIVATE libjbig)
    target_link_libraries(pbmtojbg libjbig)
    list(APPEND JBIGKIT_TARGETS pbmtojbg)

    add_executable(pbmtojbg85 src/bin/pbmtojbg85.c)
    target_include_directories(pbmtojbg85 PRIVATE libjbig85)
    target_link_libraries(pbmtojbg85 libjbig85)
    list(APPEND JBIGKIT_TARGETS pbmtojbg85)

    add_executable(jbgtopbm src/bin/jbgtopbm.c)
    target_include_directories(jbgtopbm PRIVATE libjbig)
    target_link_libraries(jbgtopbm libjbig)
    list(APPEND JBIGKIT_TARGETS jbgtopbm)

    add_executable(jbgtopbm85 src/bin/jbgtopbm85.c)
    target_include_directories(jbgtopbm85 PRIVATE libjbig85)
    target_link_libraries(jbgtopbm85 libjbig85)
    list(APPEND JBIGKIT_TARGETS jbgtopbm85)
endif()


# MARK: - 5. config.cmake Generation
include(CMakePackageConfigHelpers)

configure_package_config_file(
    ${PACKAGE_CONFIGNAME}-config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PACKAGE_CONFIGNAME}-config.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${EXPORT_NAME}
    PATH_VARS INCLUDE_INSTALL_DIR LIB_INSTALL_DIR BIN_INSTALL_DIR
)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/${PACKAGE_CONFIGNAME}-config-version.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY AnyNewerVersion
)


# MARK: - 6. Targets
install(
    TARGETS ${JBIGKIT_TARGETS}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/${PACKAGE_CONFIGNAME}-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PACKAGE_CONFIGNAME}-config-version.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${EXPORT_NAME}
)