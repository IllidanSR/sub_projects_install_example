#---------------------SET COMPONENT NAME---------------------------------------#
set(LIB_NAME ICOL.MES.applicationLayer.lib_b)
#------------------------------------------------------------------------------#
#---------------------SET LOCAL NAME TO LINK COMPONENT-------------------------#
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
set(CMAKE_CXX_STANDARD 20)
#------------------------------------------------------------------------------#
#---------------------SET PROTO FILE-------------------------------------------#
#------------------------------------------------------------------------------#
#-----------------------INCLUDE HEADER-----------------------------------------#

set(LIB_PATH ${PROJECT_SOURCE_DIR}/src/applicationLayer/lib_b/include)
set(HEADERS ${LIB_PATH}/lib_b.hpp)
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
set(SOURCE  lib_b.cpp)
#------------------------------------------------------------------------------#
#---------------------ADD LIBRARY----------------------------------------------#
add_library(${LIB_NAME} SHARED
        ${SOURCE}
        ${HEADERS})
#------------------------------------------------------------------------------#
#--------------------INCLUDE DIRS----------------------------------------------#
include_directories(${})
target_include_directories(${LIB_NAME} PUBLIC
        ${PROJECT_SOURCE_DIR}/include)
#---------------------LINK COMPONENTS------------------------------------------#
#------------------------------------------------------------------------------#
#--------------------------REQUIRED STD----------------------------------------#
target_compile_features(${LIB_NAME} PUBLIC cxx_std_20)
#------------------------------------------------------------------------------#
#-----------------------------CMAKE BUILD--------------------------------------#
source_group(TREE "${PROJECT_SOURCE_DIR}/src/applicationLayer/lib_b"
        PREFIX "Header Files"
        FILES ${HEADERS})
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