include_directories(${EXTRA_INCDIR}
    ${EXPAT_INCLUDE_DIRS}
    ${FFTW3F_INCLUDE_DIRS}
    ${GLIB2_INCLUDE_DIRS}
    ${GLIBMM_INCLUDE_DIRS}
    ${GOBJECT_INCLUDE_DIRS}
    ${GTHREAD_INCLUDE_DIRS}
    ${GTKMM_INCLUDE_DIRS}
    ${GTK_INCLUDE_DIRS}
    ${IPTCDATA_INCLUDE_DIRS}
    ${LCMS_INCLUDE_DIRS}
    ${LENSFUN_INCLUDE_DIRS}
    ${RSVG_INCLUDE_DIRS}
)

link_directories("${PROJECT_SOURCE_DIR}/rtexif"
    ${EXPAT_LIBRARY_DIRS}
    ${EXTRA_LIBDIR}
    ${FFTW3F_LIBRARY_DIRS}
    ${GLIB2_LIBRARY_DIRS}
    ${GLIBMM_LIBRARY_DIRS}
    ${GOBJECT_LIBRARY_DIRS}
    ${GTHREAD_LIBRARY_DIRS}
    ${IPTCDATA_LIBRARY_DIRS}
    ${LCMS_LIBRARY_DIRS}
    ${LENSFUN_LIBRARY_DIRS}
    ${RSVG_LIBRARY_DIRS}
)

set(CAMCONSTSFILE "camconst.json")

set(RTENGINESOURCEFILES
    ahd_demosaic_RT.cc
    amaze_demosaic_RT.cc
    badpixels.cc
    CA_correct_RT.cc
    calc_distort.cc
    camconst.cc
    capturesharpening.cc
    cfa_linedn_RT.cc
    ciecam02.cc
    cieimage.cc
    cJSON.c
    clutstore.cc
    color.cc
    colortemp.cc
    coord.cc
    cplx_wavelet_dec.cc
    curves.cc
    dcp.cc
    dcraw.cc
    dcrop.cc
    demosaic_algos.cc
    dfmanager.cc
    diagonalcurves.cc
    dirpyr_equalizer.cc
    dual_demosaic_RT.cc
    dynamicprofile.cc
    eahd_demosaic.cc
    EdgePreservingDecomposition.cc
    fast_demo.cc
    ffmanager.cc
    filmnegativeproc.cc
    filmnegativethumb.cc
    flatcurves.cc
    FTblockDN.cc
    gamutwarning.cc
    gauss.cc
    green_equil_RT.cc
    guidedfilter.cc
    hilite_recon.cc
    histmatching.cc
    hphd_demosaic_RT.cc
    iccjpeg.cc
    iccstore.cc
    iimage.cc
    image16.cc
    image8.cc
    imagedata.cc
    imagedimensions.cc
    imagefloat.cc
    imageio.cc
    improccoordinator.cc
    improcfun.cc
    impulse_denoise.cc
    init.cc
    ipdehaze.cc
    iplab2rgb.cc
    iplabregions.cc
    iplocalcontrast.cc
    ipresize.cc
    ipretinex.cc
    ipshadowshighlights.cc
    ipsharpen.cc
    ipsoftlight.cc
    iptransform.cc
    ipvibrance.cc
    ipwavelet.cc
    jdatasrc.cc
    jpeg_ijg/jpeg_memsrc.cc
    klt/convolve.cc
    klt/error.cc
    klt/klt_util.cc
    klt/klt.cc
    klt/pnmio.cc
    klt/pyramid.cc
    klt/selectGoodFeatures.cc
    klt/storeFeatures.cc
    klt/trackFeatures.cc
    klt/writeFeatures.cc
    labimage.cc
    lcp.cc
    lj92.c
    loadinitial.cc
    myfile.cc
    pdaflinesfilter.cc
    PF_correct_RT.cc
    pipettebuffer.cc
    pixelshift.cc
    previewimage.cc
    processingjob.cc
    procparams.cc
    profilestore.cc
    rawimage.cc
    rawimagesource.cc
    rcd_demosaic.cc
    refreshmap.cc
    rt_algo.cc
    rtlensfun.cc
    rtthumbnail.cc
    shmap.cc
    simpleprocess.cc
    slicer.cc
    stdimagesource.cc
    tmo_fattal02.cc
    utils.cc
    vng4_demosaic_RT.cc
    xtrans_demosaic.cc
)

if(LENSFUN_HAS_LOAD_DIRECTORY)
    set_source_files_properties(rtlensfun.cc PROPERTIES COMPILE_DEFINITIONS RT_LENSFUN_HAS_LOAD_DIRECTORY)
endif()

if(WITH_BENCHMARK)
    add_definitions(-DBENCHMARK)
endif()

if(NOT WITH_SYSTEM_KLT)
    set(RTENGINESOURCEFILES ${RTENGINESOURCEFILES}
        klt/convolve.cc
        klt/error.cc
        klt/klt_util.cc
        klt/klt.cc
        klt/pnmio.cc
        klt/pyramid.cc
        klt/selectGoodFeatures.cc
        klt/storeFeatures.cc
        klt/trackFeatures.cc
        klt/writeFeatures.cc
    )
    set(KLT_LIBRARIES)
endif()

include_directories(BEFORE "${CMAKE_CURRENT_BINARY_DIR}")

add_library(rtengine STATIC ${RTENGINESOURCEFILES})

add_dependencies(rtengine UpdateInfo)

# It may be nice to store library version too
if(BUILD_SHARED_LIBS)
    install(TARGETS rtengine DESTINATION ${LIBDIR})
endif()

set_target_properties(rtengine PROPERTIES COMPILE_FLAGS "${RTENGINE_CXX_FLAGS}")

target_link_libraries(rtengine rtexif
    ${EXPAT_LIBRARIES}
    ${EXTRA_LIB}
    ${FFTW3F_LIBRARIES}
    ${GLIB2_LIBRARIES}
    ${GLIBMM_LIBRARIES}
    ${GOBJECT_LIBRARIES}
    ${GTHREAD_LIBRARIES}
    ${IPTCDATA_LIBRARIES}
    ${JPEG_LIBRARIES}
    ${LCMS_LIBRARIES}
    ${PNG_LIBRARIES}
    ${TIFF_LIBRARIES}
    ${ZLIB_LIBRARIES}
    ${LENSFUN_LIBRARIES}
    ${RSVG_LIBRARIES}
)

install(FILES ${CAMCONSTSFILE} DESTINATION "${DATADIR}" PERMISSIONS OWNER_WRITE OWNER_READ GROUP_READ WORLD_READ)
