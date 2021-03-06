#-----------------------------------------------------------------------------
#
#  CMake Config
#
#  Libosmium benchmarks
#
#-----------------------------------------------------------------------------

message(STATUS "Configuring benchmarks")

set(BENCHMARKS
    count
    count_tag
    index_map
    mercator
    static_vs_dynamic_index
    write_pbf
    CACHE STRING "Benchmark programs"
)


#-----------------------------------------------------------------------------
#
#  Configure benchmarks
#
#-----------------------------------------------------------------------------

message(STATUS "Configuring benchmarks - Building these benchmarks:")
foreach(benchmark ${BENCHMARKS})
    message(STATUS "  - osmium_benchmark_${benchmark}")
    add_executable(osmium_benchmark_${benchmark}
                   "osmium_benchmark_${benchmark}.cpp")
    target_link_libraries(osmium_benchmark_${benchmark}
                          ${OSMIUM_IO_LIBRARIES}
                          ${BENCHMARK_LIBS_${benchmark}})
    configure_file(run_benchmark_${benchmark}.sh
                   ${CMAKE_CURRENT_BINARY_DIR}/run_benchmark_${benchmark}.sh
                   @ONLY)
endforeach()

string(TOUPPER "${CMAKE_BUILD_TYPE}" _cmake_build_type)
set(_cxx_flags "${CMAKE_CXX_FLAGS_${_cmake_build_type}}")
foreach(file setup run_benchmarks)
    configure_file(${file}.sh ${CMAKE_CURRENT_BINARY_DIR}/${file}.sh @ONLY)
endforeach()


#-----------------------------------------------------------------------------
message(STATUS "Configuring benchmarks - done")


#-----------------------------------------------------------------------------
