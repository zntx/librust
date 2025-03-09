# CMakeLists.txt
cmake_minimum_required(VERSION 3.10)

# 设置项目名称
project(librust VERSION 1.0.0)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 添加头文件目录
add_library(librust INTERFACE)
target_include_directories(librust
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

# 安装头文件
install(
    DIRECTORY include/
    DESTINATION include
)

# 安装库目标
install(
    TARGETS librust
    EXPORT librustTargets
    INCLUDES DESTINATION include
)

# 导出 CMake 配置文件
install(
    EXPORT librustTargets
    FILE librustTargets.cmake
    NAMESPACE librust::
    DESTINATION lib/cmake/librust
)

# 生成 CMake 配置文件
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/librustConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/librustConfig.cmake"
    INSTALL_DESTINATION lib/cmake/librust
)

# 安装 CMake 配置文件
install(
    FILES
        "${CMAKE_CURRENT_BINARY_DIR}/librustConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/librustConfigVersion.cmake"
    DESTINATION lib/cmake/librust
)
