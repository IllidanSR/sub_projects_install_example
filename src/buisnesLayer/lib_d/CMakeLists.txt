#---------------------SET COMPONENT NAME---------------------------------------#
set(LIB_NAME ICOL.MES.applicationLayer.lib_d)
project(${LIB_NAME}
        VERSION 1.0.1
        DESCRIPTION "lib_d")
#------------------------------------------------------------------------------#
#---------------------SET LOCAL NAME TO LINK COMPONENT-------------------------#
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
set(CMAKE_CXX_STANDARD 20)
include(GNUInstallDirs)
#------------------------------------------------------------------------------#
#---------------------SET PROTO FILE-------------------------------------------#
#------------------------------------------------------------------------------#
#-----------------------INCLUDE HEADER-----------------------------------------#

#set(LIB_PATH include)
set(HEADERS include/lib_d.hpp)
#------------------------------------------------------------------------------#
#---------------------FIND PACKAGES--------------------------------------------#
find_package(Threads REQUIRED)
#------------------------------------------------------------------------------#
#---------------------GENERATE AUTOCODE----------------------------------------#
#------------------------------------------------------------------------------#
#---------------------COMPILE DEFINES------------------------------------------#
add_definitions(-fcoroutines)
add_definitions(-fno-exceptions)
add_definitions(-fexceptions)
#------------------------------------------------------------------------------#
#---------------------ADD SOURCE-----------------------------------------------#
set(SOURCE  lib_d.cpp)
#------------------------------------------------------------------------------#
#---------------------ADD LIBRARY----------------------------------------------#
add_library(${LIB_NAME} SHARED
        ${SOURCE})

set_target_properties(${LIB_NAME} PROPERTIES
                    VERSION ${PROJECT_VERSION}
                    SOVERSION 1
                    PUBLIC_HEADER include/lib_d.hpp)

configure_file(lib_d.pc.in lib_d.pc @ONLY)
#------------------------------------------------------------------------------#
#--------------------INCLUDE DIRS----------------------------------------------#
include_directories(${})
target_include_directories(${LIB_NAME} PRIVATE .)
#---------------------LINK COMPONENTS------------------------------------------#
#------------------------------------------------------------------------------#
#--------------------------REQUIRED STD----------------------------------------#
target_compile_features(${LIB_NAME} PUBLIC cxx_std_20)
#------------------------------------------------------------------------------#
#-----------------------------CMAKE BUILD--------------------------------------#
install(TARGETS ${LIB_NAME} LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/lib_d.pc
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)
#---------------------------PVS STUDIO ANALYZE---------------------------------#
#[[if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()
if (CMAKE_BUILD_TYPE MATCHES Debug)
    pvs_studio_add_target(TARGET ${LIB_NAME}.analyze ALL
            ANALYZE ${LIB_NAME}
            OUTPUT FORMAT errorfile
            LOG ${LIB_NAME}.lib.err)
elseif (CMAKE_BUILD_TYPE MATCHES Release)
    message(build without pvs analyze)
endif ()]]
#------------------------------------------------------------------------------#