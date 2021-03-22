#---------------------SET COMPONENT NAME---------------------------------------#
set(PROJECT ICOL.MES.controller.application.main)
#------------------------------------------------------------------------------#
#---------------------SET LOCAL NAME TO LINK COMPONENT-------------------------#
if(CMAKE_HOST_WIN32)
    set(libconfig "libconfig")
else()
    set(libconfig "config")
endif()
set(LOG_LIB p7)
set(KAFKA_C rdkafka)
set(KAFKA_CPP rdkafka++)
set(KAFKA_CPP_HIGH_LVL_API CppKafka::cppkafka)
set(MANAGER_LIB ICOL.MES.controller.businessLayer.TaskManager)
set(SERVICE_LIB ICOL.MES.controller.applicationLayer.serviceMainFrame)
set(CONSUMER_LIB ICOL.MES.controller.applicationLayer.transport.consumer_)
set(FUNCTIONS_LIB ICOL.MES.controller.businessLayer.HandlerFunctions)



#------------------------------------------------------------------------------#
#-----------------------INCLUDE HEADER-----------------------------------------#
#------------------------------------------------------------------------------#
#---------------------SET PROTO FILE-------------------------------------------#
#------------------------------------------------------------------------------#
#---------------------FIND PACKAGES--------------------------------------------#
find_package(Threads REQUIRED)
find_package(CppKafka REQUIRED)
#------------------------------------------------------------------------------#
#-----------------------INCLUDE HEADER-----------------------------------------#
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()
if (CMAKE_BUILD_TYPE MATCHES Release)
    # include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
endif ()


#------------------------------------------------------------------------------#
#---------------------GENERATE AUTOCODE----------------------------------------#
#------------------------------------------------------------------------------#
#---------------------ADD SOURCE-----------------------------------------------#
set(SERVER ${PROJECT})
set(SERVER_SOURCE main.cpp)
#------------------------------------------------------------------------------#
#---------------------ADD DEFINITION-------------------------------------------#
add_definitions(-fcoroutines)
add_definitions(-fno-exceptions)
add_definitions(-fexceptions)
#------------------------------------------------------------------------------#
#--------------------INCLUDE DIRS----------------------------------------------#
#------------------------------------------------------------------------------#
#---------------------ADD EXECUTE----------------------------------------------#
add_executable(${SERVER}
        ${SERVER_SOURCE})
#------------------------------------------------------------------------------#
#--------------------------REQUIRED STD----------------------------------------#
target_compile_features(${SERVER} PRIVATE cxx_std_20)
#------------------------------------------------------------------------------#
#---------------------LINK COMPONENTS------------------------------------------#
target_link_libraries(${PROJECT} PUBLIC ICOL.MES.applicationLayer.lib_c)
#------------------------------------------------------------------------------#
#---------------------------SANITAIZERS----------------------------------------#
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()
if (CMAKE_BUILD_TYPE MATCHES Debug)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=leak -g")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address -g")
    target_link_libraries(${PROJECT} PRIVATE asan)
endif ()
#------------------------------------------------------------------------------#
#---------------------------PVS STUDIO ANALYZE---------------------------------#
