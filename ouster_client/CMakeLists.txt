

CMAKE_MINIMUM_REQUIRED(VERSION 3.5)
# project name
PROJECT(ouster_client)

# message output
MESSAGE(STATUS "This is the BINARY dir: " ${PROJECT_BINARY_DIR})
MESSAGE(STATUS "This is the SOURCE dir: " ${PROJECT_SOURCE_DIR})

# add compile options
add_compile_options(-std=c++11)
add_compile_options(-fPIC)
add_compile_options(-pthread)
add_compile_options(-DASIO_STANDALONE)
add_compile_options(-DASIO_HAS_STD_CHRONO)
find_package(Eigen3 REQUIRED)

find_package(jsoncpp REQUIRED)

# include directories
INCLUDE_DIRECTORIES(

	${PROJECT_SOURCE_DIR}/include/ouster
	${PROJECT_SOURCE_DIR}/include/optional-lite
)

# link directories

# set library output path
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

# set the library file name
AUX_SOURCE_DIRECTORY(${PROJECT_SOURCE_DIR}/src LIB_DELTAROBOTCONTROL_SRC)

# add library
ADD_LIBRARY(ouster_dynamic SHARED ${LIB_DELTAROBOTCONTROL_SRC})
ADD_LIBRARY(ouster_static STATIC ${LIB_DELTAROBOTCONTROL_SRC})

# set library properties
SET_TARGET_PROPERTIES(ouster_dynamic PROPERTIES OUTPUT_NAME "ouster_client")
SET_TARGET_PROPERTIES(ouster_dynamic PROPERTIES VERSION 1.1 SOVERSION 1)
SET_TARGET_PROPERTIES(ouster_static PROPERTIES OUTPUT_NAME "ouster_client")

# target link libraries
TARGET_LINK_LIBRARIES(ouster_dynamic jsoncpp_lib Eigen3::Eigen)
TARGET_LINK_LIBRARIES(ouster_static jsoncpp_lib Eigen3::Eigen)
