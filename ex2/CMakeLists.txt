cmake_minimum_required(VERSION 3.10)
project(Tutorial)

add_executable(TutorialExe src/tutorial.cxx)
target_include_directories(TutorialExe PRIVATE common)