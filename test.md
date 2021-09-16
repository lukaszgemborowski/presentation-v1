---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
---

# CMake introduction

Simplify your makefiles

---

## What's CMake

 - it's a build system generator
 - multiple outputs (make, ninja, nmake, etc.)
 - managing dependencies
 - suitable for C, C++

---

## Minimal example

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)

add_executable(TutorialExe tutorial.cxx)
```

```shell
$ mkdir build
$ cd build
$ cmake .. && make
```

---

## Target options

 - include directories
 - linked libraries
 - defines
 - options

---

## Include directories

```
├── CMakeLists.txt
├── common
│   └── functions.hpp
└── src
    └── tutorial.cxx
```

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)

add_executable(TutorialExe src/tutorial.cxx)
target_include_directories(TutorialExe PRIVATE common)
```

---

## Multiple CMakeLists: layout

```
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

## Multiple CMakeLists: implementation

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)
add_subdirectory(app1)
add_subdirectory(app2)
```
app1:
```cmake
add_executable(App1 app1.cxx)
target_include_directories(App1 PRIVATE ../common)
```

app2 is the same

---

## Libraries: layout

```
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

## Libraries [1/x]

Root CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial)

add_subdirectory(common)
add_subdirectory(app)
```

---

## Libraries [2/x]

Library CMakeLists.txt

```cmake
add_library(
    common
    STATIC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/functions.cxx
)

target_include_directories(
    common
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)
```

---

## Libraries [3/x]

Application CMakeLists.txt

```cmake
add_executable(app ${CMAKE_CURRENT_SOURCE_DIR}/app.cxx)
target_link_libraries(app PRIVATE common)
```