cmake_minimum_required(VERSION 3.11)

set(TARGET l2-cv-bot)
project(${TARGET})

# Add sources
add_executable(
    ${TARGET}
    src/main.cpp
    src/Options.cpp
    src/Window.cpp
    src/Capture.cpp
)

# Configure compiler
set_property(TARGET ${TARGET} PROPERTY CXX_STANDARD 17)
set_property(TARGET ${TARGET} PROPERTY CXX_STANDARD_REQUIRED ON)

# Link OpenCV
find_package(OpenCV REQUIRED)
target_link_libraries(${TARGET} ${OpenCV_LIBS})

# Bundle executable with DLLs
if(MSVC)
    set(CMAKE_INSTALL_PREFIX ${CMAKE_BINARY_DIR})
    
    # \${BUILD_TYPE} - hack to find path to executable for current configuration (Release/Debug)
    install(CODE "
        include(BundleUtilities)
        fixup_bundle(${CMAKE_INSTALL_PREFIX}/\${BUILD_TYPE}/${TARGET}.exe \"\" ${OpenCV_LIBS})
    " COMPONENT Runtime)
endif()