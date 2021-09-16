## CMake introduction

Simplify your build system

---

## What's CMake

 - it's a build system generator                <!-- .element: class="fragment" data-fragment-index="1" -->
 - multiple outputs (make, ninja, nmake, etc.)  <!-- .element: class="fragment" data-fragment-index="2" -->
 - suitable for C, C++                          <!-- .element: class="fragment" data-fragment-index="3" -->

---

## Minimal example

structure
```
├── CMakeLists.txt
└── tutorial.cxx
```

CMakeLists.txt:
<!-- .element: class="fragment" data-fragment-index="1" -->
```cmake [|1|2|3]
cmake_minimum_required(VERSION 3.10)
project(Tutorial)
add_executable(TutorialExe tutorial.cxx)
```
<!-- .element: class="fragment" data-fragment-index="1" -->

---

## Building

```sh[1|2|3|4]
$ mkdir build
$ cd build
$ cmake ..
$ make
```

---

## Target options

 - target_include_directories() <!-- .element: class="fragment" data-fragment-index="1" -->
 - target_compile_definitions() <!-- .element: class="fragment" data-fragment-index="2" -->
 - target_compile_options()     <!-- .element: class="fragment" data-fragment-index="3" -->
 - target_compile_features()    <!-- .element: class="fragment" data-fragment-index="4" -->
 - target_link_libraries()      <!-- .element: class="fragment" data-fragment-index="5" -->

---

## Include directories

``` [1|2-3|4-5]
├── CMakeLists.txt
├── common
│   └── functions.hpp
└── src
    └── tutorial.cxx
```

```cmake [4]
cmake_minimum_required(VERSION 3.10)
project(Tutorial)
add_executable(TutorialExe src/tutorial.cxx)
target_include_directories(TutorialExe PRIVATE common)
```

---

## Target compile definitions

Definition
<!-- .element: class="fragment" data-fragment-index="1" -->
```cmake
target_compile_definitions(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```
<!-- .element: class="fragment" data-fragment-index="1" -->

Example
<!-- .element: class="fragment" data-fragment-index="2" -->
```cmake
target_compile_definitions(
    TutorialExe 
    PUBLIC -DFOO BAR BAZ=1)
```
<!-- .element: class="fragment" data-fragment-index="2" -->

---

## Target compile options

Definition
<!-- .element: class="fragment" data-fragment-index="1" -->
```cmake
target_compile_options(<target> [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```
<!-- .element: class="fragment" data-fragment-index="1" -->

Example
<!-- .element: class="fragment" data-fragment-index="2" -->
```cmake
target_compile_definitions(
    TutorialExe 
    PUBLIC -Wall -Werror)
```
<!-- .element: class="fragment" data-fragment-index="2" -->

---

## Target compile features

Definition
<!-- .element: class="fragment" data-fragment-index="1" -->
```cmake
target_compile_features(<target>
  <PRIVATE|PUBLIC|INTERFACE> <feature> [...])
```
<!-- .element: class="fragment" data-fragment-index="1" -->

Example
<!-- .element: class="fragment" data-fragment-index="2" -->
```cmake
target_compile_features(
    TutorialExe PUBLIC cxx_std_17)
```
<!-- .element: class="fragment" data-fragment-index="2" -->

---

## Target link libraries

Definition
<!-- .element: class="fragment" data-fragment-index="1" -->
```cmake
target_link_libraries(<target> ... <item>... ...)
```
<!-- .element: class="fragment" data-fragment-index="1" -->

Example
<!-- .element: class="fragment" data-fragment-index="2" -->
```cmake
target_link_libraries(TutorialExe
    PRIVATE TutoriaLib rt)
```
<!-- .element: class="fragment" data-fragment-index="2" -->

---

## Multiple CMakeLists

``` [1-3|4-6|7|8-9]
├── app1
│   ├── app1.cxx
│   └── CMakeLists.txt
├── app2
│   ├── app2.cxx
│   └── CMakeLists.txt
├── CMakeLists.txt
└── common
    └── functions.hpp
```

---

## Multiple CMakeLists

```cmake [3-4]
cmake_minimum_required(VERSION 3.10)
project(Tutorial)
add_subdirectory(app1)
add_subdirectory(app2)
```

app1: 
<!-- .element: class="fragment" data-fragment-index="1" -->
```cmake
add_executable(App1 app1.cxx)
target_include_directories(App1 PRIVATE ../common)
```
<!-- .element: class="fragment" data-fragment-index="1" -->


---

## Libraries

 - created with add_library()   <!-- .element: class="fragment" data-fragment-index="1" -->
 - static and shared libraries  <!-- .element: class="fragment" data-fragment-index="2" -->
 - object libraries             <!-- .element: class="fragment" data-fragment-index="3" -->
 - interface libraries          <!-- .element: class="fragment" data-fragment-index="4" -->

---

## Libraries

``` [|5-11|1-3|4|]
├── app
│   ├── app.cxx
│   └── CMakeLists.txt
├── CMakeLists.txt
└── common
    ├── CMakeLists.txt
    ├── include
    │   └── common
    │       └── functions.hpp
    └── src
        └── functions.cxx
```

---

## Libraries

Root CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)
add_subdirectory(common)
add_subdirectory(app)
```

---

## Libraries

Library CMakeLists.txt

```cmake [|1-5|7-11|]
add_library(
    common
    STATIC
    src/functions.cxx
)

target_include_directories(
    common
    PUBLIC
    include
)
```

---

## Libraries

Application CMakeLists.txt

```cmake
add_executable(app app.cxx)
target_link_libraries(app PRIVATE common)
```

---

## Toolchain

```cmake
set(CMAKE_SYSTEM_NAME QNX)
set(arch gcc_ntoarmv7le)
set(CMAKE_C_COMPILER ntoarmv7-gcc)
set(CMAKE_C_COMPILER_TARGET ${arch})
set(CMAKE_CXX_COMPILER ntoarmv7-g++)
set(CMAKE_CXX_COMPILER_TARGET ${arch})
```

```sh
$ cmake -DCMAKE_TOOLCHAIN_FILE=my-toolchain.cmake
```
<!-- .element: class="fragment" data-fragment-index="1" -->

---

## Variables and options

```cmake
set(compile-opts "-Wall -Werror")
...
target_compile_options(TutorialExe PRIVATE ${compile-opts})
```

---

## Variables and options

```cmake[1|2|4|5|8]
option(FAIL_ON_WARN "fail on warning" ON)
set(compile-opts "-Wall")

if (FAIL_ON_WARN)
    set(compile-opts "${compile-opts} -Werror")
endif ()

target_compile_options(TutorialExe PRIVATE ${compile-opts})
```

---

## Next steps

 - cmake built in variables                     <!-- .element: class="fragment" data-fragment-index="1" -->
 - CMAKE_BUILD_TYPE !                           <!-- .element: class="fragment" data-fragment-index="2" -->
 - install                                      <!-- .element: class="fragment" data-fragment-index="3" -->
 - testing (enable_testing() / add_test())      <!-- .element: class="fragment" data-fragment-index="4" -->
 - custom targets (eg. flash firmware)          <!-- .element: class="fragment" data-fragment-index="5" -->

---

# Q&A

---

# Thank you
## Now grab a beer and pizza! :-)