cmake_minimum_required(VERSION 3.10)
project(h264-tools)

add_executable(naluparser naluparser.c)
add_executable(yuvsbspipe yuvsbspipe.c)

install(TARGETS naluparser yuvsbspipe DESTINATION bin)

