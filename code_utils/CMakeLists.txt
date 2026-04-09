cmake_minimum_required(VERSION 3.10)
project(code_utils)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

#list(APPEND CMAKE_IGNORE_PREFIX_PATH
#        /home/cat/autoware_tomato/glog/lib/cmake/glog
#        /home/ros/autoware_tomato/glog/lib/cmake/glog
#)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(sensor_msgs REQUIRED)

find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Ceres REQUIRED)
find_package(Boost REQUIRED)

include_directories("include")

# 核心库
add_library(polynomial STATIC 
  src/math_utils/Polynomial.cpp)
target_compile_options(polynomial PRIVATE -fvisibility=hidden)
target_link_libraries(polynomial ${Boost_LIBRARIES})

add_library(pnp STATIC
    src/cv_utils/dlt/dlt.cpp
    src/cv_utils/pnp/pnp.cpp
    src/cv_utils/pnp/linearpnp.cpp
    src/cv_utils/pnp/nonlinearpnp.cpp)
target_compile_options(pnp PRIVATE -fvisibility=hidden)
target_link_libraries(pnp ${Boost_LIBRARIES} ${OpenCV_LIBS} ${CERES_LIBRARIES})

add_library(cv_utils STATIC src/cv_utils.cc)
target_compile_options(cv_utils PRIVATE -fvisibility=hidden)
target_link_libraries(cv_utils ${Boost_LIBRARIES} ${OpenCV_LIBS})

# add_executable(matIO_test src/mat_io_test.cpp )
# target_link_libraries(matIO_test ${OpenCV_LIBS})

# add_executable(sumpixel_test src/sumpixel_test.cpp )
# target_link_libraries(sumpixel_test ${OpenCV_LIBS})

install(
  DIRECTORY include/
  DESTINATION include 
)
install(
  TARGETS polynomial pnp cv_utils
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

ament_package()