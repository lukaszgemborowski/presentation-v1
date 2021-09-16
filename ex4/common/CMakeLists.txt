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