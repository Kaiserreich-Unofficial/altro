
# Set directory for documentation to be the one for the current project (not top-level project)
# See Professional CMake 26.1.1

# Install targets
#install(TARGETS fmt EXPORT AltroCppTargets)

# Add interface to altrocpp export
#set_target_properties(altrocpp_interface PROPERTIES
#  EXPORT_NAME interface
#)
#install(TARGETS altrocpp_interface EXPORT AltroCppTargets)  # legal after 3.14

# Install targets from this project
set(ALTRO_INSTALL_CMAKE_DIR ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})  # location of version and config files

install(TARGETS altro_altro2
  EXPORT AltroTargets
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  FILE_SET HEADERS DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  COMPONENT AltroRuntime
)
if (NOT BUILD_SHARED_LIBS)
  install(TARGETS altro_impl EXPORT AltroTargets COMPONENT AltroRuntime)
endif()
install(EXPORT AltroTargets
  DESTINATION ${ALTRO_INSTALL_CMAKE_DIR}
  NAMESPACE ${PROJECT_NAME}::
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/altroConfigVersion.cmake"
  COMPATIBILITY SameMajorVersion
)
configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/altroConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/altroConfig.cmake
  INSTALL_DESTINATION ${ALTRO_INSTALL_CMAKE_DIR}
  NO_SET_AND_CHECK_MACRO
  NO_CHECK_REQUIRED_COMPONENTS_MACRO
)
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/altroConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/altroConfigVersion.cmake
  DESTINATION ${ALTRO_INSTALL_CMAKE_DIR}
  COMPONENT AltroRuntime
)
