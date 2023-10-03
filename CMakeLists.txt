cmake_minimum_required(VERSION 3.27)
set(CMAKE_EXPERIMENTAL_CXX_MODULE_CMAKE_API "aa1f7df0-828a-4fcd-9afc-2dc80491aca7")
set(CMAKE_EXPERIMENTAL_CXX_MODULE_DYNDEP 1)

if(NOT SKIP_VCPKG)
  if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    include(FetchContent)
    FetchContent_Declare(
      vcpkg
      GIT_REPOSITORY https://github.com/microsoft/vcpkg.git
      GIT_TAG master
      GIT_SHALLOW TRUE
    )
    FetchContent_MakeAvailable(vcpkg)
    set(CMAKE_TOOLCHAIN_FILE
      ${vcpkg_SOURCE_DIR}/scripts/buildsystems/vcpkg.cmake
      CACHE FILEPATH "Vcpkg toolchain file"
    )
    set(VCPKG_ROOT_DIR ${vcpkg_SOURCE_DIR} CACHE PATH "Vcpkg Root Directory")
  endif()

  if(DEFINED VCPKG_ROOT_DIR)
    add_custom_target(UpdateVcpkgBaseline
      ${VCPKG_ROOT_DIR}/vcpkg x-update-baseline
    )
  endif()
endif()

project(modlib2 CXX)

find_package(modlib CONFIG REQUIRED)

add_executable(modlib2 main.cpp)
target_compile_features(modlib2 PRIVATE cxx_std_23)
set_target_properties(modlib2 PROPERTIES CXX_EXTENSIONS OFF)
target_link_libraries(modlib2 PRIVATE modlib::modlib)
