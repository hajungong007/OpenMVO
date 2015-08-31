# ----------------------------------------------------------------------------
#  ��Ŀ¼��CMake�ļ�
#
#  fengbing <fengbing123@gmail.com>
#
#  ע��CMake�������� http://www.cmake.org/
# ----------------------------------------------------------------------------

CMAKE_MINIMUM_REQUIRED (VERSION 2.8)#�趨��С��CMake�汾��

PROJECT (OpenMVO)#�趨��������

LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake") # ���ӱ���CMake�ű�Ŀ¼

INCLUDE(cmake/script_version_number.cmake REQUIRED)#������Ŀ�汾��

#����·��
SET(OPENMVO_SOURCE_DIR  ${CMAKE_CURRENT_SOURCE_DIR})
SET(OPENMVO_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR})
SET(OPENMVO_BINARY_DIR  ${PROJECT_BINARY_DIR})

#�������õĺ�
INCLUDE(cmake/UtilsMacros.cmake REQUIRED)# ����һЩ�й��ߺ�

set(CMAKE_DEBUG_POSTFIX  "d")#����Debug��lib�����ƺ�����d����openmvo_XXX100d.lib

#����libs
FIND_PACKAGE(OpenCV REQUIRED) #ͨ��CMakeϵͳ����OpenCV
FIND_PACKAGE(g2o REQUIRED) #ͨ��CMakeϵͳ����g2o
FIND_PACKAGE(Boost REQUIRED)

include(cmake/script_eigen.cmake REQUIRED)  #����Eigen
#include(cmake/script_sophus.cmake REQUIRED) #����Sophus
include(cmake/script_fast.cmake REQUIRED)   #����fast lib

#������̬�⻹�Ƕ�̬�⣬Ŀǰ������Ϊ��̬�⣬���ڸ���ΪĬ�϶�̬��
SET(BUILD_SHARED_LIBS OFF CACHE BOOL "Build shared libraries (.dll/.so) instead of static ones (.lib/.a)")
IF(BUILD_SHARED_LIBS)
	SET(CMAKE_OPENMVO_BUILD_SHARED_LIB "#define OPENMVO_BUILT_AS_DLL")
	SET(CMAKE_OPENMVO_BUILD_SHARED_LIB_ONOFF 1)
ELSE(BUILD_SHARED_LIBS)
	SET(CMAKE_OPENMVO_BUILD_SHARED_LIB "/* #define OPENMVO_BUILT_AS_DLL */")
	SET(CMAKE_OPENMVO_BUILD_SHARED_LIB_ONOFF 0)
ENDIF(BUILD_SHARED_LIBS)

#����include��link·��
INCLUDE_DIRECTORIES(".")#���ð�����ǰĿ¼

LINK_DIRECTORIES("${CMAKE_BINARY_DIR}/lib")

#����·��������lib��bin��ͬһĿ¼
SET( LIBRARY_OUTPUT_PATH ${OPENMVO_BINARY_DIR}/lib CACHE PATH "Output directory for libraries" )
SET( EXECUTABLE_OUTPUT_PATH ${OPENMVO_BINARY_DIR}/bin CACHE PATH "Output directory for applications" )

# ----------------------------------------------------------------------------
#      					������Ŀ¼
# ----------------------------------------------------------------------------
ADD_SUBDIRECTORY(3rdparty)   # ���������·��
ADD_SUBDIRECTORY(libs)        # ����C++ lib��·��

SET( BUILD_APPLICATIONS ON CACHE BOOL "If you only want the OPENMVO libraries, disable this.")
IF(BUILD_APPLICATIONS)
	ADD_SUBDIRECTORY(apps)    # ����СӦ�ó���Ŀ¼
ENDIF(BUILD_APPLICATIONS)

ADD_SUBDIRECTORY(doc) #�����ĵ�Ŀ¼

#   ���ӵ�Ԫ����
# ----------------------------------------------------------------------------
SET( BUILD_TESTING ON CACHE BOOL "Build OPENMVO tests")
IF( BUILD_TESTING)
	ADD_SUBDIRECTORY(tests)  # ���ӵ�Ԫ����
ENDIF( BUILD_TESTING)