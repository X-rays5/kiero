cmake_minimum_required(VERSION 3.16)
project(kiero)

include(get_cpm.cmake)

set(CMAKE_CXX_STANDARD 17)

add_library(${PROJECT_NAME} STATIC kiero.cpp kiero.h)
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_17)
target_include_directories(${PROJECT_NAME} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

option(KIERO_SUPPORT_D3D9 "Support D3D9" OFF)
if (KIERO_SUPPORT_D3D9)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_SUPPORT_D3D9)
endif ()

option(KIERO_SUPPORT_D3D10 "Support D3D10" OFF)
if (KIERO_SUPPORT_D3D10)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_SUPPORT_D3D10)
endif ()

option(KIERO_SUPPORT_D3D10_1 "Support D3D10.1" OFF)
if (KIERO_SUPPORT_D3D10_1)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_SUPPORT_D3D10_1)
endif ()

option(KIERO_SUPPORT_D3D11 "Support D3D11" OFF)
if (KIERO_SUPPORT_D3D11)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_SUPPORT_D3D11)
endif ()

option(KIERO_SUPPORT_D3D12 "Support D3D12" OFF)
if (KIERO_SUPPORT_D3D12)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_SUPPORT_D3D12)
endif ()

option(KIERO_SUPPORT_OPENGL "Support OpenGL" OFF)
if (KIERO_SUPPORT_OPENGL)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_SUPPORT_OPENGL)
endif ()

option(KIERO_SUPPORT_VULKAN "Support Vulkan" OFF)
if (KIERO_SUPPORT_VULKAN)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_SUPPORT_VULKAN)
endif ()

option(KIERO_USE_MINHOOK "Use MinHook" OFF)
if (KIERO_USE_MINHOOK)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_USE_MINHOOK)
endif ()

option(KIERO_USE_EXTERNAL_MINHOOK "Use external MinHook" OFF)
if (KIERO_USE_EXTERNAL_MINHOOK)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_USE_EXTERNAL_MINHOOK)
endif ()

option(KIERO_USE_POLYHOOK2 "Use PolyHook2" ON)
if (KIERO_USE_POLYHOOK2)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_USE_POLYHOOK2)
endif ()

option(KIERO_USE_EXTERNAL_POLYHOOK2 "Use external PolyHook2" OFF)
if (KIERO_USE_EXTERNAL_POLYHOOK2)
  target_compile_definitions(${PROJECT_NAME} PRIVATE KIERO_USE_EXTERNAL_POLYHOOK2)
endif ()

if (KIERO_USE_MINHOOK AND NOT KIERO_USE_EXTERNAL_MINHOOK)
  cpmaddpackage("gh:X-rays5/minhook#9735edc30fd3e42b8a0bdf42a4d4814ce640d255")
  target_link_libraries(${PROJECT_NAME} PRIVATE minhook)
elseif (KIERO_USE_POLYHOOK2 AND NOT KIERO_USE_EXTERNAL_POLYHOOK2)
  cpmaddpackage("gh:stevemk14ebr/PolyHook_2_0#71553ebf0d197e73c49365cc3ab2a6aa326b3c37")
  target_link_libraries(${PROJECT_NAME} PRIVATE PolyHook_2)
endif ()