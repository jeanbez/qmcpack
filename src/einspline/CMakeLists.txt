#//////////////////////////////////////////////////////////////////////////////////////
#// This file is distributed under the University of Illinois/NCSA Open Source License.
#// See LICENSE file in top directory for details.
#//
#// Copyright (c) 2016 Jeongnim Kim and QMCPACK developers.
#//
#// File developed by: Jeremy McMinnis, jmcminis@gmail.com, University of Illinois at Urbana-Champaign
#//		       Anouar Benali, benali@anl.gov, Argonne National Laboratory
#//		       Ye Luo, yeluo@anl.gov, Argonne National Laboratory
#//
#// File created by: Jeongnim Kim, jeongnim.kim@gmail.com, University of Illinois at Urbana-Champaign
#//////////////////////////////////////////////////////////////////////////////////////

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

set(SRCS bspline_create.c bspline_data.c multi_bspline_create.c multi_bspline_copy.c)

set(SRCS ${SRCS} bspline_eval_d_std.cpp multi_bspline_eval_s_std3.cpp multi_bspline_eval_d_std3.cpp
         multi_bspline_eval_z_std3.cpp)

if(QMC_CUDA)
  set(SRCS ${SRCS} multi_bspline_create_cuda.cu bspline_create_cuda.cu)
  if(NOT QMC_CUDA2HIP)
    add_library(einspline ${SRCS})
  else()
    hip_add_library(einspline ${SRCS})
  endif(NOT QMC_CUDA2HIP)
  target_link_libraries(einspline PRIVATE platform_runtime)
else(QMC_CUDA)
  add_library(einspline ${SRCS})
endif(QMC_CUDA)

target_compile_definitions(einspline INTERFACE "HAVE_EINSPLINE")

#ADD_EXECUTABLE(time_multi time_multi_new.c)
#target_link_libraries(time_multi einspline)
#add_dependencies(time_multi ${PROJECT_BINARY_HOME}/include/einspline/bspline.h)
#ADD_EXECUTABLE(test_multi_single test_multi_single.cpp)
#target_link_libraries(test_multi_single einspline)

if(BUILD_UNIT_TESTS)
  add_subdirectory(tests)
endif()
