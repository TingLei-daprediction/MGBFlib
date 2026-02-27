include(GNUInstallDirs)

list(APPEND ${PROJECT_NAME}_src_files
  jp_pbfil.f90
  jp_pbfil2.f90
  jp_pbfil3.f90
  jp_pietc.f90
  jp_pietc_s.f90
  jp_pkind.f90
  jp_pkind2.f90
  jp_pmat.f90
  jp_pmat4.f90
  mg_bocos.f90
  mg_domain.f90
  mg_domain_loc.f90
  mg_entrymod.f90
  mg_filtering.f90
  mg_generations.f90
  mg_interpolate.f90
  mg_intstate.f90
  mg_mppstuff.f90
  mg_parameter.f90
  mg_timers.f90
  mg_transfer.f90
  mg_tools.f90
  mgbf_kind.f90
  mgbf_util.f90
  phint.f90 
  phint1.f90
 type_intstat_locpointer.inc
 type_intstat_point2this.inc
 type_parameter_locpointer.inc
 type_parameter_point2this.inc
)



message(STATUS "PROJECT_NAME = ${PROJECT_NAME}")
message(STATUS "${PROJECT_NAME}_src_files = ${${PROJECT_NAME}_src_files}")

ecbuild_add_library( TARGET ${PROJECT_NAME}
                     SOURCES ${${PROJECT_NAME}_src_files}
                     INSTALL_HEADERS LISTED)

target_link_libraries( ${PROJECT_NAME} PUBLIC OpenMP::OpenMP_Fortran )
target_link_libraries( ${PROJECT_NAME} PUBLIC NetCDF::NetCDF_Fortran )
target_link_libraries( ${PROJECT_NAME} PUBLIC MPI::MPI_Fortran )
#clt target_link_libraries( ${PROJECT_NAME} PUBLIC ${LAPACK_LIBRARIES} )

# Enable including of local header Fortran file templates
target_include_directories( ${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..> )

# Fortran module output directory for build and install interfaces
set( MODULE_DIR module/${PROJECT_NAME}/${CMAKE_Fortran_COMPILER_ID}/${CMAKE_Fortran_COMPILER_VERSION} )
set_target_properties( ${PROJECT_NAME} PROPERTIES Fortran_MODULE_DIRECTORY ${CMAKE_BINARY_DIR}/${MODULE_DIR} )
install( DIRECTORY ${CMAKE_BINARY_DIR}/${MODULE_DIR}/ DESTINATION ${MODULE_DIR} )
target_include_directories( ${PROJECT_NAME} INTERFACE
                            $<BUILD_INTERFACE:${CMAKE_BINARY_DIR}/${MODULE_DIR}>
                            $<INSTALL_INTERFACE:${MODULE_DIR}> )

