CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

project(BIMG CXX)

set(SRC src/image.cpp
        src/image_gnf.cpp
)

set(SRC_DEC src/image_decode.cpp
)

set(SRC_ENC src/image_cubemap_filter.cpp
        src/image_encode.cpp
)

set(SRC_ASTC
        3rdparty/astc-encoder/source/astcenc_averages_and_directions.cpp
        3rdparty/astc-encoder/source/astcenc_block_sizes.cpp
        3rdparty/astc-encoder/source/astcenc_color_quantize.cpp
        3rdparty/astc-encoder/source/astcenc_color_unquantize.cpp
        3rdparty/astc-encoder/source/astcenc_compress_symbolic.cpp
        3rdparty/astc-encoder/source/astcenc_compute_variance.cpp
        3rdparty/astc-encoder/source/astcenc_decompress_symbolic.cpp
        3rdparty/astc-encoder/source/astcenc_diagnostic_trace.cpp
        3rdparty/astc-encoder/source/astcenc_entry.cpp
        3rdparty/astc-encoder/source/astcenc_find_best_partitioning.cpp
        3rdparty/astc-encoder/source/astcenc_ideal_endpoints_and_weights.cpp
        3rdparty/astc-encoder/source/astcenc_image.cpp
        3rdparty/astc-encoder/source/astcenc_integer_sequence.cpp
        3rdparty/astc-encoder/source/astcenc_mathlib.cpp
        3rdparty/astc-encoder/source/astcenc_mathlib_softfloat.cpp
        3rdparty/astc-encoder/source/astcenc_partition_tables.cpp
        3rdparty/astc-encoder/source/astcenc_percentile_tables.cpp
        3rdparty/astc-encoder/source/astcenc_pick_best_endpoint_format.cpp
        3rdparty/astc-encoder/source/astcenc_quantization.cpp
        3rdparty/astc-encoder/source/astcenc_symbolic_physical.cpp
        3rdparty/astc-encoder/source/astcenc_weight_align.cpp
        3rdparty/astc-encoder/source/astcenc_weight_quant_xfer_tables.cpp
)

set(SRC_SQUISH
        3rdparty/libsquish/alpha.cpp
        3rdparty/libsquish/clusterfit.cpp
        3rdparty/libsquish/colourblock.cpp
        3rdparty/libsquish/colourfit.cpp
        3rdparty/libsquish/colourset.cpp
        3rdparty/libsquish/maths.cpp
        3rdparty/libsquish/rangefit.cpp
        3rdparty/libsquish/singlecolourfit.cpp
        3rdparty/libsquish/squish.cpp
)

set(SRC_EDTAA3
        3rdparty/edtaa3/edtaa3func.cpp
)

set(SRC_ETC
        3rdparty/etc1/etc1.cpp
        3rdparty/etc2/ProcessRGB.cpp
        3rdparty/etc2/Tables.cpp
)

set(SRC_NVTT
        3rdparty/nvtt/nvtt.cpp
)

set(SRC_PVRTC
        3rdparty/pvrtc/BitScale.cpp
        3rdparty/pvrtc/MortonTable.cpp
        3rdparty/pvrtc/PvrTcDecoder.cpp
        3rdparty/pvrtc/PvrTcEncoder.cpp
        3rdparty/pvrtc/PvrTcPacket.cpp

)

set(SRC_IQA
        3rdparty/iqa/source/convolve.c
        3rdparty/iqa/source/decimate.c
        3rdparty/iqa/source/math_utils.c
        3rdparty/iqa/source/ms_ssim.c
        3rdparty/iqa/source/mse.c
        3rdparty/iqa/source/psnr.c
        3rdparty/iqa/source/ssim.c
)

include_directories(
        include

        # Dependencies
        ../bx/include
        3rdparty
        3rdparty/astc-encoder/include
        3rdparty/iqa/include
        3rdparty/nvtt
        3rdparty/tinyexr/deps/miniz
)

add_definitions(-DBX_CONFIG_DEBUG=0)

set(CMAKE_CXX_STANDARD 20)

add_library(bimg STATIC ${SRC} ${SRC_ASTC})
add_library(bimg_encode STATIC ${SRC_ENC})
add_library(bimg_decode STATIC ${SRC_DEC} ${SRC_SQUISH} ${SRC_EDTAA3} ${SRC_ETC} ${SRC_NVTT} ${SRC_PVRTC} ${SRC_IQA})

set_target_properties(bimg bimg_encode bimg_decode PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(bimg bimg_encode bimg_decode PROPERTIES CXX_STANDARD 20)
set_target_properties(bimg bimg_encode bimg_decode PROPERTIES CXX_STANDARD_REQUIRED ON)
set_target_properties(bimg bimg_encode bimg_decode PROPERTIES CXX_EXTENSIONS OFF)

if (WIN32)
    target_compile_options(bimg PRIVATE /Zc:__cplusplus /Zc:preprocessor)
    target_compile_options(bimg_encode PRIVATE /Zc:__cplusplus /Zc:preprocessor)
    target_compile_options(bimg_decode PRIVATE /Zc:__cplusplus /Zc:preprocessor)
    include_directories(../bx/include/compat/msvc)
endif()