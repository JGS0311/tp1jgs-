# CMake version mínima requerida
cmake_minimum_required(VERSION 3.10)

# Nombre del proyecto
project(project)

# Crea una biblioteca estática en vez de un ejecutable
add_library(so-i-24-JGS0311-otra STATIC
        src/main.c
        src/expose_metrics.c
        src/metrics.c
        ${SRC_FILES})

# Incluye el directorio de cabeceras (headers)
include_directories(include)
include_directories(./lib/prom/include)
include_directories(./lib/promhttp/include)

add_subdirectory(./lib/prom)
add_subdirectory(./lib/promhttp)

link_directories(${CMAKE_SOURCE_DIR}/lib/prom/build)
link_directories(${CMAKE_SOURCE_DIR}/lib/promhttp/build)

#target_link_libraries(so-i-24-JGS0311-otra)

#Crear ejecutable para monitoreo
add_executable(monitoreo
    ./src/main.c
    ./src/expose_metrics.c
    ./src/metrics.c
)
target_link_libraries(monitoreo PRIVATE pthread prom promhttp microhttpd)
