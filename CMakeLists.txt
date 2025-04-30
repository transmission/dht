cmake_minimum_required(VERSION 3.16.3)
project(dht C)

add_library(${PROJECT_NAME} STATIC
    dht.c
)

install(TARGETS ${PROJECT_NAME} DESTINATION lib)
install(FILES dht.h DESTINATION include/dht)
