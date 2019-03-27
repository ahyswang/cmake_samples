# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
# 项目信息
project (demo5)

# 加入一个配置头文件，用于处理cmake对源码的设置
configure_file (
	"${PROJECT_SOURCE_DIR}/config.h.in"
	"${PROJECT_BINARY_DIR}/config.h"
)

# 是否使用自己的math库
option (USE_MYMATH
		"Use provided math implementation" ON)

include_directories ("${PROJECT_BINARY_DIR}")

# 是否加入math库
if (USE_MYMATH)
	include_directories ("${PROJECT_SOURCE_DIR}/math")
	add_subdirectory(math)
	set (EXTRA_LIBS ${EXTRA_LIBS} math)
endif (USE_MYMATH)

# 查找当前目录下的所有源文件
aux_source_directory(. SRCS)

# 指定生成目标
add_executable(demo5 main.cpp)
# 添加链接库
target_link_libraries(demo5 ${EXTRA_LIBS})

# 设置安装路径
set (CMAKE_INSTALL_PREFIX /tmp/demo5)
# 指定安装路径
install (TARGETS demo5 DESTINATION bin)
install (FILES "${PROJECT_BINARY_DIR}/config.h"
         DESTINATION include)

