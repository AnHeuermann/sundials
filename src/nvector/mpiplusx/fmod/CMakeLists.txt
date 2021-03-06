# ---------------------------------------------------------------
# Programmer(s): Cody J. Balos @ LLNL
# ---------------------------------------------------------------
# SUNDIALS Copyright Start
# Copyright (c) 2002-2020, Lawrence Livermore National Security
# and Southern Methodist University.
# All rights reserved.
#
# See the top-level LICENSE and NOTICE files for details.
#
# SPDX-License-Identifier: BSD-3-Clause
# SUNDIALS Copyright End
# ---------------------------------------------------------------
# CMakeLists.txt file for the F2003 MPIPlusX NVECTOR object library
# ---------------------------------------------------------------

set(nvecmpiplusx_SOURCES fnvector_mpiplusx_mod.f90 fnvector_mpiplusx_mod.c)

if(MPI_ENABLE AND MPI_C_FOUND)
  if(MPI_C_COMPILER)
    # use MPI wrapper as the compiler
    set(CMAKE_C_COMPILER ${MPI_C_COMPILER})
  elseif()
    # add MPI_INCLUDE_PATH to include directories
    include_directories(${MPI_INCLUDE_PATH})
  endif()
  if(MPI_Fortran_COMPILER)
    # use MPI wrapper as the compiler
    set(CMAKE_Fortran_COMPILER ${MPI_Fortran_COMPILER})
  endif()
endif()

if(BUILD_STATIC_LIBS)
  if(MPI_ENABLE AND MPI_C_FOUND)
    sundials_add_f2003_interface_library(sundials_fnvecmpiplusx_mod_static STATIC_OBJECT
                                         ${nvecmpiplusx_SOURCES})
    add_dependencies(sundials_fnvecmpiplusx_mod_static_obj sundials_fgeneric_static_obj)
    set_target_properties(sundials_fnvecmpiplusx_mod_static PROPERTIES
                          OUTPUT_NAME sundials_fnvecmpiplusx_mod
                          CLEAN_DIRECT_OUTPUT 1)
    install(TARGETS sundials_fnvecmpiplusx_mod_static DESTINATION ${CMAKE_INSTALL_LIBDIR})
  endif()
endif(BUILD_STATIC_LIBS)

if(BUILD_SHARED_LIBS)
  if(MPI_ENABLE AND MPI_C_FOUND)
    sundials_add_f2003_interface_library(sundials_fnvecmpiplusx_mod_shared SHARED_OBJECT
                                         ${nvecmpiplusx_SOURCES})
    add_dependencies(sundials_fnvecmpiplusx_mod_shared_obj sundials_fgeneric_shared_obj)
    set_target_properties(sundials_fnvecmpiplusx_mod_shared PROPERTIES
                          OUTPUT_NAME sundials_fnvecmpiplusx_mod
                          CLEAN_DIRECT_OUTPUT 1)
    install(TARGETS sundials_fnvecmpiplusx_mod_shared DESTINATION ${CMAKE_INSTALL_LIBDIR})
  endif()
endif(BUILD_SHARED_LIBS)

message(STATUS "Added NVECTOR_MPIPLUSX F2003 interface")
