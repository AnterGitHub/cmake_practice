# 指定 cmake 版本，预防某些用户使用较低版本的来编译该项目
cmake_minimum_required(VERSION 3.2)

# 指定一个项目名称，后面可以通过 `${PROJECT_NAME}` 来引用
PROJECT(my_project)

# 指定头文件目录，改参数为头文件所在的文件夹名字
INCLUDE_DIRECTORIES(include)

# 源文件目录
AUX_SOURCE_DIRECTORY(src DIR_SRCS)

# 选择性设置环境变量，然后通过 ${CUSTOM_VAR} 来引用
# SET(CUSTOM_VAR ${DIR_SRCS})




###################### make executable file ######################
# 添加需要编译的源文件 test.cpp 和 输出的可执行文件名字 `${PROJECT_NAME}`
ADD_EXECUTABLE(${PROJECT_NAME} ${DIR_SRCS})




###################### static lib test ######################
# 设置一个动态库名字，因为 `ADD_LIBRARY` 中的参数不给设置字符常量
SET(DYLIB_NAME dy_test)

# 生成一个动态库
ADD_LIBRARY(${DYLIB_NAME}
            SHARED
            src/u.cpp include/u.h)

# 添加需要编译的源文件 test.cpp 和 输出的可执行文件名字 exe_main
ADD_EXECUTABLE(exe_main_dy src/test.cpp)

# 连接
TARGET_LINK_LIBRARIES(exe_main_dy ${DYLIB_NAME})




###################### static lib test ######################
# 设置一个动态库名字，因为 `ADD_LIBRARY` 中的参数不给设置字符常量
SET(STATIC_LIB_NAME static_test)

# 生成一个动态库
ADD_LIBRARY(${STATIC_LIB_NAME}
            STATIC
            src/u.cpp include/u.h)

# 添加需要编译的源文件 test.cpp 和 输出的可执行文件名字 exe_main
ADD_EXECUTABLE(exe_main_static src/test.cpp)

# 连接
TARGET_LINK_LIBRARIES(exe_main_static ${STATIC_LIB_NAME})




###################### link lib test ######################
# 设置一个动态库名字，因为 `ADD_LIBRARY` 中的参数不给设置字符常量
SET(LINK_LIBS
	${CMAKE_CURRENT_LIST_DIR}/lib/libstatic_test.a
# 	${CMAKE_CURRENT_LIST_DIR}/lib/libdy_test.dylib
	)
SET(LIB_LINK_TEST_OUTPUT link_exist_lib_output)

# 添加需要编译的源文件 test.cpp 和 输出的可执行文件名字 exe_main
ADD_EXECUTABLE(${LIB_LINK_TEST_OUTPUT} 
            src/test.cpp)

# 连接
TARGET_LINK_LIBRARIES(${LIB_LINK_TEST_OUTPUT} 
                    ${LINK_LIBS})

