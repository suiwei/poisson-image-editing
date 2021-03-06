cmake_minimum_required(VERSION 2.8)

project(poissonblend)

if (WIN32)
	#set(OpenCV_STATIC true)
	add_definitions("-D_SCL_SECURE_NO_WARNINGS")
endif()

set(EIGEN_INCLUDE_DIR "../eigen" CACHE PATH "Where is the include directory of Eigen located")
set(BLEND_TESTS_VERBOSE 0 CACHE BOOL "Tests will show visualizations when enabled")
find_package(OpenCV REQUIRED)

include_directories(${CMAKE_CURRENT_BINARY_DIR} ${OpenCV_INCLUDE_DIRS} ${EIGEN_INCLUDE_DIR} "inc")

# Library

add_library(blend 
	inc/blend/poisson_solver.h
	inc/blend/clone.h
	inc/blend/blend.h
	src/poisson_solver.cpp
	src/clone.cpp	
	src/blend.cpp
)
	
target_link_libraries(blend ${OpenCV_LIBRARIES})
	
# Samples

add_executable(seamless_cloning examples/seamless_cloning.cpp)
target_link_libraries(seamless_cloning blend ${OpenCV_LIBRARIES})

add_executable(seamless_blending examples/seamless_blending.cpp)
target_link_libraries(seamless_blending blend ${OpenCV_LIBRARIES})

# Tests

configure_file(tests/config.h.in test_config.h)
add_executable(tests tests/catch.hpp tests/poisson.cpp)
target_link_libraries(tests blend ${OpenCV_LIBRARIES})