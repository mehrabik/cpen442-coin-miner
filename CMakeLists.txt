cmake_minimum_required(VERSION 3.16)
project(cpen442_coin_miner)

# links to resources because I'm learning CMake
# https://cmake.org/cmake/help/latest/guide/tutorial/index.html
# https://cliutils.gitlab.io/modern-cmake/
# https://github.com/lefticus/cpp_weekly_game_project
# cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
# cmake --build build

set(CMAKE_CXX_STANDARD 20 CACHE STRING "The C++ standard to use")
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)
set(CMAKE_CXX_EXTENSIONS FALSE)
if(MSVC)
	add_compile_options(/W4 /WX)
else()
	add_compile_options(
		-Wall -Wextra -Wpedantic #-Werror
		-Wold-style-cast -Wcast-qual
		-Wformat=2
		-Wimplicit-fallthrough #=5 apple clang rejects this :/
		-Wshadow
		-march=native
	)
endif()

set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)

find_package(Threads REQUIRED)
set(OPENSSL_USE_STATIC_LIBS)
find_package(OpenSSL REQUIRED)

# https://cliutils.gitlab.io/modern-cmake/chapters/testing.html
if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
	include(CTest)
endif()

add_subdirectory(src)

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME AND BUILD_TESTING)
	# add_subdirectory(tests)
endif()
