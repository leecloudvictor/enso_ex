cmake_minimum_required(VERSION 2.8)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{VITASDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

project(mmt)
include("${VITASDK}/share/vita.cmake" REQUIRED)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wl,-q -Wall -O3 -nostdlib")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti -fno-exceptions")

add_executable(mmt
  main.c
)

target_link_libraries(mmt
  SceIofilemgrForDriver_stub
  SceSysclibForDriver_stub
  SceSysmemForDriver_stub
  SceModulemgrForKernel_stub
  SceModulemgrForDriver_stub
  SceThreadmgrForDriver_stub
  SceKernelSuspendForDriver_stub
  taihenForKernel_stub
  taihenModuleUtils_stub
)

vita_create_self(mmt.skprx mmt CONFIG exports.yml UNSAFE)

vita_create_stubs(stubs mmt ${CMAKE_SOURCE_DIR}/exports.yml KERNEL)
