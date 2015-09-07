# -DCMAKE_BUILD_TYPE=Debug
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
project( SceneViewer )

SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/CMakeModules/")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++11 -fext-numeric-literals")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")

# Overide with cmake -DCMAKE_BUILD_TYPE=Debug {dir}
IF( NOT CMAKE_BUILD_TYPE )
   SET( CMAKE_BUILD_TYPE Release )
ENDIF()

# Platform configuration vars
INCLUDE(SetPlatformVars)

FIND_PACKAGE(GLEW REQUIRED)
FIND_PACKAGE(Cg REQUIRED)
FIND_PACKAGE(Eigen3 REQUIRED)
FIND_PACKAGE(OpenCV2 REQUIRED)

INCLUDE(${PROJECT_SOURCE_DIR}/CMakeModules/Find_SSE.cmake)

SET(
  SOURCES
./common/Assets.cpp
./common/Assets.h
./common/Camera.cpp
./common/Camera.h
./common/Common.h
./common/FreeImage.h
./common/Matrix4.cpp
./common/Matrix4.h
./common/Mesh.cpp
./common/Mesh.h
./common/Model.cpp
./common/ModelDatabase.cpp
./common/ModelDatabase.h
./common/Model.h
./common/ModelInstance.cpp
./common/ModelInstance.h
./common/Parameters.cpp
./common/Parameters.h
./common/Scene.cpp
./common/Scene.h
./common/Utility.cpp
./common/Utility.h
./common/Vec3f.h
)

INCLUDE_DIRECTORIES(
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_SOURCE_DIR}
        ${CMAKE_CURRENT_BINARY_DIR}
)

INCLUDE_DIRECTORIES(
  ${OpenCV2_INCLUDE_PATH}
  ${CG_INCLUDE_PATH}
  ${EIGEN3_INCLUDE_DIR}
  ${OPENCV_INCLUDE_DIRS}
)

LINK_DIRECTORIES(
  ${OPENCV_LIBRARY_DIRS}
  /usr/local/lib
  /usr/local/lib64
)


LINK_LIBRARIES(
  ${CG_LIBRARY}
  ${CG_GL_LIBRARY}
  ${Eigen3_LIBRARY}
  ${OpenCV_LIBS}
  -lglut
   /usr/lib/x86_64-linux-gnu/mesa/libGL.so
  -lGLU
  -lX11
  -lXmu
  -lXi
  -lm
   /usr/local/lib/libopencv_core.so
   /usr/local/lib/libopencv_features2d.so
   /home/ankur/workspace/code/stanford-scene-database/common/FreeImage/lib/linux/x86_64/libfreeimage.a
)

IF(OPENMP_FOUND)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
ENDIF()

## The compiler option "--use_fast_math" forces the compiler to convert
## function(x) into __function(x) that has higher throughput.

#set (CMAKE_RUNTIME_OUTPUT_DIRECTORY "bin/")

ADD_EXECUTABLE(sceneViewer
	      ${SOURCES}
	     ./sceneViewer/main.cpp
 	     ./sceneViewer/sceneViewer.cpp
 	     ./sceneViewer/sceneViewer.h)
