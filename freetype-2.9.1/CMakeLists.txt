#最小版本号
CMAKE_MINIMUM_REQUIRED(VERSION 3.4.1)

set(tf_inc_dir ./include)
set(tf_lib_name "typefree")

include_directories(
        ./include
)


set(BASE_SRCS
        src/autofit/autofit.c
        src/base/ftbase.c
        src/base/ftbbox.c
        src/base/ftbdf.c
        src/base/ftbitmap.c
        src/base/ftcid.c
        src/base/ftfstype.c
        src/base/ftgasp.c
        src/base/ftglyph.c
        src/base/ftgxval.c
        src/base/ftinit.c
        src/base/ftmm.c
        src/base/ftotval.c
        src/base/ftpatent.c
        src/base/ftpfr.c
        src/base/ftstroke.c
        src/base/ftsynth.c
        src/base/ftsystem.c
        src/base/fttype1.c
        src/base/ftwinfnt.c
        src/bdf/bdf.c
        src/bzip2/ftbzip2.c
        src/cache/ftcache.c
        src/cff/cff.c
        src/cid/type1cid.c
        src/gzip/ftgzip.c
        src/lzw/ftlzw.c
        src/pcf/pcf.c
        src/pfr/pfr.c
        src/psaux/psaux.c
        src/pshinter/pshinter.c
        src/psnames/psnames.c
        src/raster/raster.c
        src/sfnt/sfnt.c
        src/smooth/smooth.c
        src/truetype/truetype.c
        src/type1/type1.c
        src/type42/type42.c
        src/winfonts/winfnt.c
        )

SET(CMAKE_C_FLAGS  "-DFT2_BUILD_LIBRARY")


add_library(
        ${tf_lib_name}
        STATIC
        ${BASE_SRCS}
)

install(TARGETS ${tf_lib_name} DESTINATION lib)
install(FILES ${tf_inc_dir}/ft2build.h DESTINATION include/)