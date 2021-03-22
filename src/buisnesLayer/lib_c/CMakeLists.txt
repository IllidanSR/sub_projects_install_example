#---------------------SET COMPONENT NAME---------------------------------------#
set(LIB_NAME ICOL.MES.applicationLayer.lib_c)


#------------------------------------------------------------------------------#
#---------------------SET LOCAL NAME TO LINK COMPONENT-------------------------#
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")
set(CMAKE_CXX_STANDARD 20)
#------------------------------------------------------------------------------#
#---------------------SET PROTO FILE-------------------------------------------#
#------------------------------------------------------------------------------#
#-----------------------INCLUDE HEADER-----------------------------------------#
set(LIB_D ICOL.MES.applicationLayer.lib_d)
set(LIB_PATH ${PROJECT_SOURCE_DIR}/src/buisnesLayer/lib_c/include)
set(HEADERS ${LIB_PATH}/lib_c.hpp)
message(${PROJECT_SOURCE_DIR})
#------------------------------------------------------------------------------#
#---------------------FIND PACKAGES--------------------------------------------#
find_package(Threads REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(ICOL.MES.applicationLayer.lib_d lib_d)
#------------------------------------------------------------------------------#
#---------------------GENERATE AUTOCODE----------------------------------------#
#------------------------------------------------------------------------------#
#---------------------COMPILE DEFINES------------------------------------------#
add_definitions(-fcoroutines)
add_definitions(-fno-exceptions)
add_definitions(-fexceptions)
#------------------------------------------------------------------------------#
#---------------------ADD SOURCE-----------------------------------------------#
set(SOURCE  lib_c.cpp)
#------------------------------------------------------------------------------#
#---------------------ADD LIBRARY----------------------------------------------#
add_library(${LIB_NAME} SHARED
        ${SOURCE}
        ${HEADERS})
#------------------------------------------------------------------------------#
#--------------------INCLUDE DIRS----------------------------------------------#
include_directories(${})
target_include_directories(${LIB_NAME} PUBLIC
        ${PROJECT_SOURCE_DIR}/src/buisnesLayer)
#---------------------LINK COMPONENTS------------------------------------------#
target_link_libraries(${LIB_NAME} PRIVATE ${LIB_D})
#------------------------------------------------------------------------------#
#--------------------------REQUIRED STD----------------------------------------#
target_compile_features(${LIB_NAME} PUBLIC cxx_std_20)
#------------------------------------------------------------------------------#
#-----------------------------CMAKE BUILD--------------------------------------#
source_group(TREE "${PROJECT_SOURCE_DIR}/src/buisnesLayer/lib_c"
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