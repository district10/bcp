bcp
===

Standalone version of bcp does not require you having BOOST configured,
You need only a C++98 compatable compiler, and cmake that can generate
proper Makefile or other project file.

## Linux

```bash
mkdir build && cd build
cmake ..
make
```

## Window

Use cmake-gui to build Visual Studio project. (not work by now)

---

If you are curious about how to get this standalone pack,
here's several simple steps to follow.

## 0. get bcp binary & boost source file

bcp:

-   Windows: <http://whudoc.qiniudn.com/2016/bcp.exe> (565 KB)
-   Linux: <http://whudoc.qiniudn.com/2016/bcp_standalone_linux> (2.28 MB)

boost source:

-   goto official [Boost Downloads](http://www.boost.org/users/download/) to get a copy, or use my archive
-   <http://whudoc.qiniudn.com/2016/boost_1_58_0_headers_sources.7z> (37.6 MB)

## 1. grab relative headers and source files

```bash
# upzip boost source archive
# cd boost source dir
mkdir outputs
bcp \
    boost/detail/lightweight_main.hpp \
    boost/detail/workaround.hpp \
    boost/filesystem/exception.hpp \
    boost/filesystem/fstream.hpp \
    boost/filesystem/operations.hpp \
    boost/filesystem/path.hpp \
    boost/lexical_cast.hpp \
    boost/regex.hpp \
    boost/shared_ptr.hpp \
    boost/throw_exception.hpp \
    boost/version.hpp \
    outputs
```

## 2. rip out irrelative build scripts in `libs` dir & copy to your project

-   `build` dirs and `test(s)` dirs under `libs` are irrelative
-   copy `boost` & `libs` to `CMake_Source_Dir`

## 3. modify CMakeLists.txt

add these two lines to your root CMakeLists.txt

```cmake
include_directories( ${CMAKE_SOURCE_DIR} )
add_subdirectory( libs )
```

add a CMakeLists.txt to the grabbed-out `libs` dir, containing

```cmake
project( MiniBoost )
file( GLOB_RECURSE LIBSRCS *.c *.cpp *.cc *.h *.hpp )
add_library( ${PROJECT_NAME} STATIC ${LIBSRCS} )
```

at last, you can link them to your binary

```cmake
target_link_libraries( ${SOME_EXECUTABLE} MiniBoost )
```

You can checkout [CMakeLists.txt](CMakeLists.txt) for a real example.
