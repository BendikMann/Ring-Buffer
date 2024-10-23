cmake_minimum_required(VERSION 3.15)
project(RingBuffer LANGUAGES C VERSION 1.0.1)

# make cache variables for install destinations
include(GNUInstallDirs)

# specify the C Standard
set(CMAKE_C_STANDARD 99)

# Create Library
add_library(RingBuffer src/ringbuffer.c)

# add include directories
target_include_directories(RingBuffer
        PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/includes>"
        "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/includes>"
)
# Install RingBuffer into upstream developers libraries
install(TARGETS RingBuffer
        EXPORT RingBufferTargets
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(FILES includes/ringbuffer.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(EXPORT RingBufferTargets
        FILE RingBufferTargets.cmake
        NAMESPACE RingBuffer::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/RingBuffer
)

# Create Package

include(CMakePackageConfigHelpers)

configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/cmake/Config.cmake.in
        "${CMAKE_CURRENT_BINARY_DIR}/RingBufferConfig.cmake"
        INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/RingBuffer
)

install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/RingBufferConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/RingBufferConfigVersion.cmake"
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/RingBuffer
)

set(version 1.0.1)

set_property(TARGET RingBuffer PROPERTY VERSION ${version})
set_property(TARGET RingBuffer PROPERTY SOVERSION 1)
set_property(TARGET RingBuffer PROPERTY
        INTERFACE_RingBuffer_MAJOR_VERSION 3)
set_property(TARGET RingBuffer APPEND PROPERTY
        COMPATIBLE_INTERFACE_STRING RingBuffer_MAJOR_VERSION
)

# generate the version file for the config file
write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/RingBufferConfigVersion.cmake"
        VERSION "${version}"
        COMPATIBILITY AnyNewerVersion
)


enable_testing()
add_subdirectory(examples)