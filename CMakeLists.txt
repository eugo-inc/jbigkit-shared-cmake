# MARK: - 1. Preface
cmake_minimum_required(VERSION 3.28)
project(jbigkit VERSION 2.5.5 LANGUAGES C)


# MARK: - 2. General
set(CMAKE_C_STANDARD 17)

include_directories(src/include)


# MARK: - 3. Libraries
add_library(libjbig SHARED src/lib/jbig.c src/lib/jbig_ar.c)
set_target_properties(libjbig PROPERTIES PUBLIC_HEADER "src/include/jbig.h;src/include/jbig_ar.h")
set_target_properties(libjbig PROPERTIES VERSION 0.0.0 SOVERSION 0)
set_target_properties(libjbig PROPERTIES OUTPUT_NAME jbig)

add_library(libjbig85 SHARED src/lib/jbig85.c src/lib/jbig_ar.c)
set_target_properties(libjbig85 PROPERTIES PUBLIC_HEADER "src/include/jbig85.h;src/include/jbig_ar.h")
set_target_properties(libjbig85 PROPERTIES VERSION 0.0.0 SOVERSION 0)
set_target_properties(libjbig85 PROPERTIES OUTPUT_NAME jbig85)

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


# MARK: - 5. Targets Installation
install(
    TARGETS ${JBIGKIT_TARGETS}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
