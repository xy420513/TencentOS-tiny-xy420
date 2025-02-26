cmake_minimum_required(VERSION 3.8)

project(ini_test)

set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

set(TINY_ROOT ../../../)

include_directories(${TINY_ROOT}/osal/cmsis_os)
include_directories(${TINY_ROOT}/kernel/core/include)
include_directories(${TINY_ROOT}/kernel/evtdrv/include)
include_directories(${TINY_ROOT}/kernel/hal/include)
include_directories(${TINY_ROOT}/kernel/pm/include)

set(CMSIS_SRCS ${TINY_ROOT}/osal/cmsis_os/cmsis_os.c)
aux_source_directory(${TINY_ROOT}/kernel/core CORE_SRCS)
aux_source_directory(${TINY_ROOT}/kernel/evtdrv EVTDRV_SRCS)
aux_source_directory(${TINY_ROOT}/kernel/pm PM_SRCS)

set(ARCH_ROOT ${TINY_ROOT}/arch/linux)

include_directories(${ARCH_ROOT}/common/include)
include_directories(${ARCH_ROOT}/posix/gcc)
include_directories(${TINY_ROOT}/components/utils/INI/include)

aux_source_directory(${ARCH_ROOT}/common ARCH_COMMON_SRCS)
aux_source_directory(${ARCH_ROOT}/posix/gcc ARCH_POSIX_SRCS)

aux_source_directory(${TINY_ROOT}/components/utils/INI/src INI_SRCS)

set(ARCH_SRCS ${ARCH_COMMON_SRCS} ${ARCH_POSIX_SRCS})

set(TINY_SRCS ${ARCH_SRCS} ${CMSIS_SRCS} ${EVTDRV_SRCS} ${PM_SRCS} ${CORE_SRCS} ${INI_SRCS} )

include_directories(./)
include_directories(./inc)

set(APP_SRCS src/main.c)

add_executable(inidemo ${APP_SRCS} ${TINY_SRCS})

target_link_libraries(inidemo pthread)