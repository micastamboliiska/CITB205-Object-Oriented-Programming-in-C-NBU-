cmake_minimum_required(VERSION 3.5)
project(lab1)

set(CMAKE_CXX_STANDARD 11)

set(SOURCE_FILES src/main.cpp src/product.cpp src/item.cpp src/invoice.cpp src/textprinter.cpp)
add_executable(lab2 ${SOURCE_FILES})
