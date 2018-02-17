cmake_minimum_required(VERSION 3.5)

project(nanovg)

add_library(${PROJECT_NAME}
    src/nanovg.c
)

# Install the different headers and libraries.
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/fontstash.h
              ${CMAKE_CURRENT_SOURCE_DIR}/src/nanovg_gl_utils.h
              ${CMAKE_CURRENT_SOURCE_DIR}/src/nanovg_gl.h
              ${CMAKE_CURRENT_SOURCE_DIR}/src/nanovg.h
              ${CMAKE_CURRENT_SOURCE_DIR}/src/stb_image.h
              ${CMAKE_CURRENT_SOURCE_DIR}/src/stb_truetype.h
        DESTINATION include/nanovg COMPONENT Development
)

# Create the CMake version file.
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/nanovgConfigVersion.cmake"
  VERSION 2017.12.19
  COMPATIBILITY AnyNewerVersion
)

set(targets_export_name "nanovgTargets")

# Create the Config file.
include(CMakePackageConfigHelpers)
set(ConfigPackageLocation lib/cmake/nanovg)
configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/nanovgConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/nanovgConfig.cmake
  INSTALL_DESTINATION ${ConfigPackageLocation}
)

install(TARGETS ${PROJECT_NAME}
      EXPORT ${targets_export_name}
      LIBRARY DESTINATION lib COMPONENT Runtime
      ARCHIVE DESTINATION lib COMPONENT Development
      RUNTIME DESTINATION bin COMPONENT Runtime
      PUBLIC_HEADER DESTINATION include COMPONENT Development
      BUNDLE DESTINATION bin COMPONENT Runtime
)

# Install the generated CMake files.
install(EXPORT ${targets_export_name} NAMESPACE "nanovg::" DESTINATION lib/cmake/nanovg)
install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/nanovgConfigVersion.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/nanovgConfig.cmake"
  DESTINATION ${ConfigPackageLocation}
)
