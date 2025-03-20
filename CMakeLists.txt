cmake_minimum_required(VERSION 3.31)

project(libcamera)

# set default build type
# https://blog.kitware.com/cmake-and-the-default-build-type/
if((NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES) OR (CMAKE_BUILD_TYPE STREQUAL "None"))
  set(default_build_type "Release")
  message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release")
endif()

string(TOLOWER ${CMAKE_BUILD_TYPE} MESON_BUILD_TYPE)

if(NOT NJOBS)
  set(NJOBS 0)
endif()

find_package(OpenSSL REQUIRED)

include(ExternalProject)

ExternalProject_Add(libcamera_source
  GIT_REPOSITORY      https://github.com/raspberrypi/libcamera.git
  GIT_TAG             v0.6.0+rpt20251127
  GIT_CONFIG          advice.detachedHead=false
  GIT_SHALLOW         ON
  CONFIGURE_COMMAND   meson setup build ../libcamera_source --prefix ${CMAKE_INSTALL_PREFIX} --libdir lib --buildtype ${MESON_BUILD_TYPE} --auto-features=disabled
                       -Dpipelines=all -Dipas=rpi/vc4,rpi/pisp -Dv4l2=true -Dgstreamer=enabled -Dtest=false -Dlc-compliance=disabled -Dcam=enabled -Dqcam=disabled -Ddocumentation=disabled -Dpycamera=enabled
  BUILD_COMMAND       meson compile -C build --jobs ${NJOBS}
  INSTALL_COMMAND     meson install -C build
)

add_library(libcamera SHARED IMPORTED)
add_dependencies(libcamera libcamera_source)
