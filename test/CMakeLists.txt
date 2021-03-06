anydsl_runtime_wrap(STENCIL_INT_PROGRAM
    CLANG_FLAGS ${CLANG_FLAGS}
    IMPALA_FLAGS ${IMPALA_FLAGS}
    FILES stencil_dsl.impala)
add_executable(stencil_dsl ${STENCIL_INT_PROGRAM})
target_link_libraries(stencil_dsl ${ANYDSL_RUNTIME_LIBRARY} ${ANYDSL_RUNTIME_LIBRARIES})
add_test("stencil_dsl" stencil_dsl)

if(NOT (BACKEND STREQUAL "cpu" OR BACKEND STREQUAL "avx"))
    anydsl_runtime_wrap(MULTI_GPU_PROGRAM
        CLANG_FLAGS ${CLANG_FLAGS}
        IMPALA_FLAGS ${IMPALA_FLAGS}
        FILES ../${BACKEND_FILE} ../utils.impala multi_gpu.impala)
    add_executable(multi_gpu ${MULTI_GPU_PROGRAM})
    target_link_libraries(multi_gpu ${ANYDSL_RUNTIME_LIBRARY} ${ANYDSL_RUNTIME_LIBRARIES})
    add_test("multi_gpu" multi_gpu)

    anydsl_runtime_wrap(ALLOC_GPU_PROGRAM
        CLANG_FLAGS ${CLANG_FLAGS}
        IMPALA_FLAGS ${IMPALA_FLAGS}
        FILES ../${BACKEND_FILE} alloc_gpu.impala)
    add_executable(alloc_gpu ${ALLOC_GPU_PROGRAM})
    target_link_libraries(alloc_gpu ${ANYDSL_RUNTIME_LIBRARY} ${ANYDSL_RUNTIME_LIBRARIES})
    add_test("alloc_gpu" alloc_gpu)

    anydsl_runtime_wrap(ALLOC_GPU_UNIFIED_PROGRAM
        CLANG_FLAGS ${CLANG_FLAGS}
        IMPALA_FLAGS ${IMPALA_FLAGS}
        FILES ../${BACKEND_FILE} alloc_gpu_unified.impala)
    add_executable(alloc_gpu_unified ${ALLOC_GPU_UNIFIED_PROGRAM})
    target_link_libraries(alloc_gpu_unified ${ANYDSL_RUNTIME_LIBRARY} ${ANYDSL_RUNTIME_LIBRARIES})
    add_test("alloc_gpu_unified" alloc_gpu_unified)

    anydsl_runtime_wrap(RESERVE_SHARED_PROGRAM
        CLANG_FLAGS ${CLANG_FLAGS}
        IMPALA_FLAGS ${IMPALA_FLAGS}
        FILES ../${BACKEND_FILE} reserve_shared.impala)
    add_executable(reserve_shared ${RESERVE_SHARED_PROGRAM})
    target_link_libraries(reserve_shared ${ANYDSL_RUNTIME_LIBRARY} ${ANYDSL_RUNTIME_LIBRARIES})
    add_test("reserve_shared" reserve_shared)

    if(${CMAKE_SYSTEM_PROCESSOR} MATCHES "aarch64")
        anydsl_runtime_wrap(HALF_FLOAT_GPU_PROGRAM
            CLANG_FLAGS ${CLANG_FLAGS}
            IMPALA_FLAGS ${IMPALA_FLAGS}
            FILES ../${BACKEND_FILE} half_float_gpu.impala)
        add_executable(half_float_gpu ${HALF_FLOAT_GPU_PROGRAM})
        target_link_libraries(half_float_gpu ${ANYDSL_RUNTIME_LIBRARY} ${ANYDSL_RUNTIME_LIBRARIES})
        add_test("half_float_gpu" half_float_gpu)
    endif()
endif()
