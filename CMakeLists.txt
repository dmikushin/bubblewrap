cmake_minimum_required(VERSION 3.10)
project(bwrap VERSION 0.10.0 LANGUAGES C)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

add_definitions(-D_GNU_SOURCE)

include_directories(.)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Werror=shadow -Werror=empty-body -Werror=strict-prototypes -Werror=missing-prototypes -Werror=implicit-function-declaration -Werror=pointer-arith -Werror=init-self -Werror=missing-declarations -Werror=return-type -Werror=overflow -Werror=int-conversion -Werror=parentheses -Werror=incompatible-pointer-types -Werror=misleading-indentation -Werror=missing-include-dirs -Werror=aggregate-return -Werror=switch-default -Wswitch-enum -Wno-missing-field-initializers -Wno-error=missing-field-initializers")

find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBCAP REQUIRED libcap)
pkg_check_modules(LIBSELINUX REQUIRED libselinux)

add_library(${PROJECT_NAME} STATIC src/bwrap.c src/bind-mount.c src/network.c src/utils.c)
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)
target_link_libraries(${PROJECT_NAME} ${LIBCAP_LIBRARIES} ${LIBSELINUX_LIBRARIES})
target_include_directories(${PROJECT_NAME} PRIVATE ${LIBCAP_INCLUDE_DIRS} ${LIBSELINUX_INCLUDE_DIRS})
target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_SELINUX HAVE_SELINUX_2_3)
target_compile_options(${PROJECT_NAME} PRIVATE ${LIBCAP_CFLAGS_OTHER} ${LIBSELINUX_CFLAGS_OTHER})

add_executable(${PROJECT_NAME}_cli src/main.c)
target_link_libraries(${PROJECT_NAME}_cli ${PROJECT_NAME})

install(TARGETS bwrap DESTINATION bin)

