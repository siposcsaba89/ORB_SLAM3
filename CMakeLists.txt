cmake_minimum_required(VERSION 3.5)
project(ORB_SLAM3)
set(CMAKE_CXX_STAMDARD 14)
set(CMAKE_CXX_STAMDARD_REQUIRED TRUE)


IF(NOT CMAKE_BUILD_TYPE)
  SET(CMAKE_BUILD_TYPE Release)
ENDIF()

MESSAGE(STATUS "Build type: " ${CMAKE_BUILD_TYPE})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall   -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall   -O3")
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -march=native")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -march=native")

find_package(OpenCV REQUIRED)
find_package(Eigen3 3.1.0 REQUIRED CONFIG)
find_package(Pangolin REQUIRED CONFIG)
find_package(g2o CONFIG REQUIRED)
find_package(DBoW2 REQUIRED)
set(Boost_USE_STATIC_LIBS        OFF) # only find static libs
set(Boost_USE_MULTITHREADED      ON)
set(Boost_USE_STATIC_RUNTIME    OFF)
find_package(Boost REQUIRED COMPONENTS serialization)
message(STATUS "slm as${DBoW2_LIBRARIES}")


add_library(${PROJECT_NAME} STATIC
	src/System.cc
	src/Tracking.cc
	src/LocalMapping.cc
	src/LoopClosing.cc
	src/ORBextractor.cc
	src/ORBmatcher.cc
	src/FrameDrawer.cc
	src/Converter.cc
	src/MapPoint.cc
	src/KeyFrame.cc
	src/Atlas.cc
	src/Map.cc
	src/MapDrawer.cc
	src/Optimizer.cc
	src/PnPsolver.cc
	src/Frame.cc
	src/KeyFrameDatabase.cc
	src/Sim3Solver.cc
	src/Initializer.cc
	src/Viewer.cc
	src/ImuTypes.cc
	src/G2oTypes.cc
	src/CameraModels/Pinhole.cpp
	src/CameraModels/KannalaBrandt8.cpp
	src/OptimizableTypes.cpp
	src/MLPnPsolver.cpp
	src/Random.h
	src/Random.cpp
	src/Timestamp.h
	src/Timestamp.cpp
	include/System.h
	include/Tracking.h
	include/LocalMapping.h
	include/LoopClosing.h
	include/ORBextractor.h
	include/ORBmatcher.h
	include/FrameDrawer.h
	include/Converter.h
	include/MapPoint.h
	include/KeyFrame.h
	include/Atlas.h
	include/Map.h
	include/MapDrawer.h
	include/Optimizer.h
	include/PnPsolver.h
	include/Frame.h
	include/KeyFrameDatabase.h
	include/Sim3Solver.h
	include/Initializer.h
	include/Viewer.h
	include/ImuTypes.h
	include/G2oTypes.h
	include/CameraModels/GeometricCamera.h
	include/CameraModels/Pinhole.h
	include/CameraModels/KannalaBrandt8.h
	include/OptimizableTypes.h
	include/MLPnPsolver.h
	include/TwoViewReconstruction.h
	src/TwoViewReconstruction.cc
)

target_include_directories(${PROJECT_NAME} PUBLIC
	${PROJECT_SOURCE_DIR}
	${PROJECT_SOURCE_DIR}/include
	${PROJECT_SOURCE_DIR}/include/CameraModels
	${EIGEN3_INCLUDE_DIR}
	${Pangolin_INCLUDE_DIRS}
)

target_link_libraries(${PROJECT_NAME} PUBLIC
	
	pangolin
	${OpenCV_LIBRARIES}
	Eigen3::Eigen
	g2o::core g2o::stuff g2o::types_icp g2o::types_sba
	g2o::types_slam2d g2o::types_slam3d g2o::types_sba g2o::types_sim3 g2o::solver_dense g2o::solver_eigen g2o::types_data
	${DBoW2_LIBRARIES}
	#${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
	#${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
	#-lboost_serialization
	#-lcrypto
	Boost::disable_autolinking Boost::dynamic_linking Boost::serialization Boost::boost
)

target_include_directories(${PROJECT_NAME} PUBLIC ${DBoW2_INCLUDE_DIRS})
target_compile_options(${PROJECT_NAME} PUBLIC /W3 /bigobj)
target_compile_definitions(${PROJECT_NAME} PRIVATE -DCOMPILEDWITHC11)
# Build examples

add_executable(rgbd_tum
Examples/RGB-D/rgbd_tum.cc)
target_link_libraries(rgbd_tum ${PROJECT_NAME})
target_compile_definitions(rgbd_tum PRIVATE -DCOMPILEDWITHC11)


add_executable(stereo_kitti
Examples/Stereo/stereo_kitti.cc)
target_link_libraries(stereo_kitti ${PROJECT_NAME})
target_compile_definitions(stereo_kitti PRIVATE -DCOMPILEDWITHC11)


add_executable(stereo_euroc
Examples/Stereo/stereo_euroc.cc)
target_link_libraries(stereo_euroc ${PROJECT_NAME})
target_compile_definitions(stereo_euroc PRIVATE -DCOMPILEDWITHC11)


add_executable(stereo_tum_vi
Examples/Stereo/stereo_tum_vi.cc)
target_link_libraries(stereo_tum_vi ${PROJECT_NAME})
target_link_libraries(stereo_tum_vi ${PROJECT_NAME})
target_compile_definitions(stereo_tum_vi PRIVATE -DCOMPILEDWITHC11)


add_executable(mono_tum
Examples/Monocular/mono_tum.cc)
target_link_libraries(mono_tum ${PROJECT_NAME})
target_compile_definitions(mono_tum PRIVATE -DCOMPILEDWITHC11)


add_executable(mono_kitti
Examples/Monocular/mono_kitti.cc)
target_link_libraries(mono_kitti ${PROJECT_NAME})
target_compile_definitions(mono_kitti PRIVATE -DCOMPILEDWITHC11)


add_executable(mono_euroc
Examples/Monocular/mono_euroc.cc)
target_link_libraries(mono_euroc ${PROJECT_NAME})
target_compile_definitions(mono_euroc PRIVATE -DCOMPILEDWITHC11)


add_executable(mono_tum_vi
Examples/Monocular/mono_tum_vi.cc)
target_link_libraries(mono_tum_vi ${PROJECT_NAME})
target_compile_definitions(mono_tum_vi PRIVATE -DCOMPILEDWITHC11)

add_executable(mono_inertial_euroc
Examples/Monocular-Inertial/mono_inertial_euroc.cc)
target_link_libraries(mono_inertial_euroc ${PROJECT_NAME})
target_compile_definitions(mono_inertial_euroc PRIVATE -DCOMPILEDWITHC11)


add_executable(mono_inertial_tum_vi
Examples/Monocular-Inertial/mono_inertial_tum_vi.cc)
target_link_libraries(mono_inertial_tum_vi ${PROJECT_NAME})
target_compile_definitions(mono_inertial_tum_vi PRIVATE -DCOMPILEDWITHC11)


add_executable(stereo_inertial_euroc
Examples/Stereo-Inertial/stereo_inertial_euroc.cc)
target_link_libraries(stereo_inertial_euroc ${PROJECT_NAME})
target_compile_definitions(stereo_inertial_euroc PRIVATE -DCOMPILEDWITHC11)


add_executable(stereo_inertial_tum_vi
Examples/Stereo-Inertial/stereo_inertial_tum_vi.cc)
target_link_libraries(stereo_inertial_tum_vi ${PROJECT_NAME})
target_compile_options(stereo_inertial_tum_vi PUBLIC /W3 /bigobj)
target_compile_definitions(stereo_inertial_tum_vi PRIVATE -DCOMPILEDWITHC11)


