cmake_minimum_required (VERSION 2.6.2)
project (PETSc C)

include (${PETSC_CMAKE_ARCH}/lib/petsc/conf/PETScBuildInternal.cmake)

if (PETSC_HAVE_FORTRAN)
  enable_language (Fortran)
endif ()
if (PETSC_CLANGUAGE_Cxx OR PETSC_HAVE_CXX)
  enable_language (CXX)
endif ()

if (APPLE)
  SET(CMAKE_C_ARCHIVE_FINISH "<CMAKE_RANLIB> -c <TARGET> ")
  SET(CMAKE_CXX_ARCHIVE_FINISH "<CMAKE_RANLIB> -c <TARGET> ")
  SET(CMAKE_Fortran_ARCHIVE_FINISH "<CMAKE_RANLIB> -c <TARGET> ")
endif ()

if (PETSC_HAVE_CUDA)
  find_package (CUDA REQUIRED)
  set (CUDA_PROPAGATE_HOST_FLAGS OFF)
  set (CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS} --compiler-options ${PETSC_CUDA_HOST_FLAGS})
endif ()

include_directories ("${PETSc_SOURCE_DIR}/include" "${PETSc_BINARY_DIR}/include")

set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PETSc_BINARY_DIR}/lib" CACHE PATH "Output directory for PETSc archives")
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PETSc_BINARY_DIR}/lib" CACHE PATH "Output directory for PETSc libraries")
set (CMAKE_Fortran_MODULE_DIRECTORY "${PETSc_BINARY_DIR}/include" CACHE PATH "Output directory for fortran *.mod files")
mark_as_advanced (CMAKE_ARCHIVE_OUTPUT_DIRECTORY CMAKE_LIBRARY_OUTPUT_DIRECTORY CMAKE_Fortran_MODULE_DIRECTORY)
set (CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")
set (CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

###################  The following describes the build  ####################

include_directories (${PETSC_PACKAGE_INCLUDES})
if (PETSC_HAVE_RANDOM123)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/random/impls/random123/random123.c
    )
endif ()
list (APPEND PETSCSYS_SRCS
  src/sys/info/verboseinfo.c
  src/sys/logging/plog.c
  src/sys/logging/xmllogevent.c
  src/sys/logging/xmlviewer.c
  src/sys/time/cputime.c
  src/sys/time/fdate.c
  src/sys/objects/version.c
  src/sys/objects/gcomm.c
  src/sys/objects/gtype.c
  src/sys/objects/olist.c
  src/sys/objects/pname.c
  src/sys/objects/tagm.c
  src/sys/objects/destroy.c
  src/sys/objects/gcookie.c
  src/sys/objects/inherit.c
  src/sys/objects/options.c
  src/sys/objects/pgname.c
  src/sys/objects/prefix.c
  src/sys/objects/init.c
  src/sys/objects/pinit.c
  src/sys/objects/ptype.c
  src/sys/objects/state.c
  src/sys/objects/aoptions.c
  src/sys/objects/subcomm.c
  src/sys/objects/fcallback.c
  src/sys/python/pythonsys.c
  src/sys/utils/arch.c
  src/sys/utils/fhost.c
  src/sys/utils/fuser.c
  src/sys/utils/memc.c
  src/sys/utils/mpiu.c
  src/sys/utils/psleep.c
  src/sys/utils/sortd.c
  src/sys/utils/sorti.c
  src/sys/utils/str.c
  src/sys/utils/sortip.c
  src/sys/utils/pbarrier.c
  src/sys/utils/pdisplay.c
  src/sys/utils/ctable.c
  src/sys/utils/psplit.c
  src/sys/utils/mpimesg.c
  src/sys/utils/sseenabled.c
  src/sys/utils/mpitr.c
  src/sys/utils/mpilong.c
  src/sys/utils/mathinf.c
  src/sys/utils/matheq.c
  src/sys/utils/mpits.c
  src/sys/utils/segbuffer.c
  src/sys/utils/mpishm.c
  src/sys/memory/mal.c
  src/sys/memory/mem.c
  src/sys/memory/mtr.c
  src/sys/memory/mhbw.c
  src/sys/dll/dlimpl.c
  src/sys/dll/dl.c
  src/sys/dll/reg.c
  src/sys/totalview/tv_data_display.c
  src/sys/classes/draw/impls/tikz/tikz.c
  src/sys/classes/draw/impls/image/drawimage.c
  src/sys/classes/draw/impls/null/drawnull.c
  src/sys/classes/draw/interface/draw.c
  src/sys/classes/draw/interface/dcoor.c
  src/sys/classes/draw/interface/dtext.c
  src/sys/classes/draw/interface/dpoint.c
  src/sys/classes/draw/interface/dmarker.c
  src/sys/classes/draw/interface/dline.c
  src/sys/classes/draw/interface/dpause.c
  src/sys/classes/draw/interface/dflush.c
  src/sys/classes/draw/interface/dsave.c
  src/sys/classes/draw/interface/dclear.c
  src/sys/classes/draw/interface/dmouse.c
  src/sys/classes/draw/interface/dviewp.c
  src/sys/classes/draw/interface/dtri.c
  src/sys/classes/draw/interface/drect.c
  src/sys/classes/draw/interface/dellipse.c
  src/sys/classes/draw/interface/drawreg.c
  src/sys/classes/draw/interface/drawregall.c
  src/sys/classes/draw/utils/axis.c
  src/sys/classes/draw/utils/lg.c
  src/sys/classes/draw/utils/dscatter.c
  src/sys/classes/draw/utils/hists.c
  src/sys/classes/draw/utils/zoom.c
  src/sys/classes/draw/utils/cmap.c
  src/sys/classes/draw/utils/lgc.c
  src/sys/classes/draw/utils/axisc.c
  src/sys/classes/draw/utils/bars.c
  src/sys/classes/draw/utils/image.c
  src/sys/classes/gll/petscgll.c
  src/sys/classes/viewer/impls/binary/binv.c
  src/sys/classes/viewer/impls/draw/drawv.c
  src/sys/classes/viewer/impls/vu/petscvu.c
  src/sys/classes/viewer/impls/vtk/vtkv.c
  src/sys/classes/viewer/impls/glvis/glvis.c
  src/sys/classes/viewer/impls/ascii/filev.c
  src/sys/classes/viewer/impls/ascii/vcreatea.c
  src/sys/classes/viewer/impls/string/stringv.c
  src/sys/classes/viewer/interface/view.c
  src/sys/classes/viewer/interface/flush.c
  src/sys/classes/viewer/interface/viewregall.c
  src/sys/classes/viewer/interface/viewreg.c
  src/sys/classes/viewer/interface/viewa.c
  src/sys/classes/viewer/interface/dlregispetsc.c
  src/sys/classes/viewer/interface/viewers.c
  src/sys/classes/viewer/interface/dupl.c
  src/sys/classes/random/impls/rander48/rander48.c
  src/sys/classes/random/interface/random.c
  src/sys/classes/random/interface/randreg.c
  src/sys/classes/random/interface/dlregisrand.c
  src/sys/classes/random/interface/randomc.c
  src/sys/classes/bag/bag.c
  src/sys/fileio/ftest.c
  src/sys/fileio/ghome.c
  src/sys/fileio/mpiuopen.c
  src/sys/fileio/rpath.c
  src/sys/fileio/fpath.c
  src/sys/fileio/fwd.c
  src/sys/fileio/grpath.c
  src/sys/fileio/mprint.c
  src/sys/fileio/sysio.c
  src/sys/fileio/fretrieve.c
  src/sys/fileio/smatlab.c
  src/sys/fileio/fdir.c
  src/sys/error/adebug.c
  src/sys/error/err.c
  src/sys/error/errtrace.c
  src/sys/error/errabort.c
  src/sys/error/errstop.c
  src/sys/error/fp.c
  src/sys/error/signal.c
  src/sys/error/pstack.c
  src/sys/error/checkptr.c
  )
if (PETSC_HAVE_MATLAB_ENGINE)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/matlabengine/matlab.c
    src/sys/classes/viewer/impls/matlab/vmatlab.c
    )
endif ()
if (PETSC_HAVE_MPIUNI)
  list (APPEND PETSCSYS_SRCS
    src/sys/mpiuni/mpi.c
    src/sys/mpiuni/mpitime.c
    )
endif ()
if (PETSC_HAVE_DRAND48)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/random/impls/rand48/rand48.c
    )
endif ()
if (PETSC_HAVE_OPENGL)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/draw/impls/opengl/openglops.c
    )
endif ()
if (PETSC_HAVE_SPRNG)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/random/impls/sprng/sprng.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_HDF5)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/hdf5/ftn-custom/zhdf5f.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_YAML)
  list (APPEND PETSCSYS_SRCS
    src/sys/yaml/ftn-custom/zyamlimplsf.c
    )
endif ()
if (PETSC_USE_WINDOWS_GRAPHICS)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/draw/impls/win32/win32draw.c
    )
endif ()
if (PETSC_USE_SOCKET_VIEWER AND PETSC_HAVE_SSL)
  list (APPEND PETSCSYS_SRCS
    src/sys/webclient/client.c
    src/sys/webclient/google.c
    src/sys/webclient/box.c
    src/sys/webclient/textbelt.c
    src/sys/webclient/globus.c
    src/sys/webclient/tellmycell.c
    )
endif ()
if (PETSC_HAVE_ADIOS)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/adios/adios.c
    )
endif ()
if (PETSC_HAVE_X)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/draw/impls/x/xinit.c
    src/sys/classes/draw/impls/x/ximage.c
    src/sys/classes/draw/impls/x/xcolor.c
    src/sys/classes/draw/impls/x/xops.c
    src/sys/classes/draw/impls/x/xioerr.c
    src/sys/classes/draw/impls/x/xtext.c
    src/sys/classes/draw/impls/x/xtone.c
    src/sys/classes/draw/impls/x/drawopenx.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_MPIUNI)
  list (APPEND PETSCSYS_SRCS
    src/sys/mpiuni/fsrc/somempifort.F
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_X)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/draw/impls/x/ftn-custom/zdrawopenxf.c
    )
endif ()
if (PETSC_USE_LOG)
  list (APPEND PETSCSYS_SRCS
    src/sys/logging/utils/classlog.c
    src/sys/logging/utils/stagelog.c
    src/sys/logging/utils/eventlog.c
    src/sys/logging/utils/stack.c
    )
endif ()
if (PETSC_HAVE_ADIOS2)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/adios2/adios2.c
    )
endif ()
if (PETSC_USE_FORTRAN_KERNELS)
  list (APPEND PETSCSYS_SRCS
    src/sys/utils/ftn-kernels/fcopy.F
    )
endif ()
if (PETSC_USE_SOCKET_VIEWER)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/socket/send.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_MATLAB_ENGINE)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/matlabengine/ftn-custom/zmatlabf.c
    src/sys/classes/viewer/impls/matlab/ftn-custom/zvmatlabf.c
    )
endif ()
if (PETSC_HAVE_MATHEMATICA AND NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/mathematica/mathematica.c
    )
endif ()
if (PETSC_HAVE_HDF5)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/hdf5/hdf5v.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90 AND PETSC_HAVE_MPIUNI)
  list (APPEND PETSCSYS_SRCS
    src/sys/mpiuni/f90-mod/mpiunimod.F
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USE_SOCKET_VIEWER)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/socket/ftn-custom/zsendf.c
    )
endif ()
if (PETSC_HAVE_YAML)
  list (APPEND PETSCSYS_SRCS
    src/sys/yaml/yamlimpls.c
    )
endif ()
if (PETSC_HAVE_RAND)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/random/impls/rand/rand.c
    )
endif ()
if (PETSC_HAVE_SAWS)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/ams/ams.c
    src/sys/classes/viewer/impls/ams/amsopen.c
    src/sys/ams/pams.c
    )
endif ()
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCSYS_SRCS
    src/sys/info/ftn-custom/zverboseinfof.c
    src/sys/ftn-custom/zsys.c
    src/sys/ftn-custom/zutils.c
    src/sys/logging/ftn-custom/zplogf.c
    src/sys/time/ftn-custom/zptimef.c
    src/sys/objects/ftn-custom/zgcommf.c
    src/sys/objects/ftn-custom/zgtype.c
    src/sys/objects/ftn-custom/zoptionsf.c
    src/sys/objects/ftn-custom/zpgnamef.c
    src/sys/objects/ftn-custom/zpnamef.c
    src/sys/objects/ftn-custom/zprefixf.c
    src/sys/objects/ftn-custom/zdestroyf.c
    src/sys/objects/ftn-custom/zstart.c
    src/sys/objects/ftn-custom/zstartf.c
    src/sys/objects/ftn-custom/zversionf.c
    src/sys/objects/ftn-custom/zinheritf.c
    src/sys/objects/ftn-custom/zptypef.c
    src/sys/python/ftn-custom/zpythonf.c
    src/sys/utils/ftn-custom/zarchf.c
    src/sys/utils/ftn-custom/zstrf.c
    src/sys/utils/ftn-custom/zfhostf.c
    src/sys/memory/ftn-custom/zmtrf.c
    src/sys/classes/draw/interface/ftn-custom/zdrawf.c
    src/sys/classes/draw/interface/ftn-custom/zdrawregf.c
    src/sys/classes/draw/interface/ftn-custom/zdtextf.c
    src/sys/classes/draw/interface/ftn-custom/zdtrif.c
    src/sys/classes/draw/utils/ftn-custom/zaxisf.c
    src/sys/classes/draw/utils/ftn-custom/zlgcf.c
    src/sys/classes/draw/utils/ftn-custom/zzoomf.c
    src/sys/classes/viewer/impls/binary/ftn-custom/zbinvf.c
    src/sys/classes/viewer/impls/draw/ftn-custom/zdrawvf.c
    src/sys/classes/viewer/impls/vtk/ftn-custom/zvtkvf.c
    src/sys/classes/viewer/impls/ascii/ftn-custom/zfilevf.c
    src/sys/classes/viewer/impls/ascii/ftn-custom/zvcreatef.c
    src/sys/classes/viewer/impls/string/ftn-custom/zstringvf.c
    src/sys/classes/viewer/interface/ftn-custom/zviewaf.c
    src/sys/classes/viewer/interface/ftn-custom/zviewasetf.c
    src/sys/classes/random/interface/ftn-custom/zrandomf.c
    src/sys/classes/bag/ftn-custom/zbagf.c
    src/sys/fileio/ftn-custom/zghomef.c
    src/sys/fileio/ftn-custom/zmpiuopenf.c
    src/sys/fileio/ftn-custom/zmprintf.c
    src/sys/fileio/ftn-custom/zsysiof.c
    src/sys/error/ftn-custom/zerrf.c
    src/sys/fsrc/somefort.F
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F2003)
  list (APPEND PETSCSYS_SRCS
    src/sys/objects/f2003-src/fsrc/optionenum.F
    src/sys/classes/bag/f2003-src/fsrc/bagenum.F
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCSYS_SRCS
    src/sys/classes/viewer/impls/binary/f90-custom/zbinvf90.c
    src/sys/classes/bag/f90-custom/zbagf90.c
    src/sys/f90-mod/petscsysmod.F
    src/sys/f90-src/f90_cwrap.c
    src/sys/f90-src/fsrc/f90_fwrap.F
    )
endif ()
if (PETSC_USE_SOCKET_VIEWER AND PETSC_USE_MATLAB_SOCKET AND PETSC_USE_REAL_DOUBLE AND NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCSYS_SRCS
    
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petscsys ${PETSCSYS_SRCS})
  else ()
    add_library (petscsys ${PETSCSYS_SRCS})
  endif ()
  target_link_libraries (petscsys  ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petscsys PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petscsys PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()
if (PETSC_HAVE_CUDA AND PETSC_HAVE_VIENNACL)
  list (APPEND PETSCVEC_SRCS
    src/vec/vec/impls/seq/seqviennaclcuda/vecviennaclcuda.cu
    src/vec/vec/impls/mpi/mpiviennaclcuda/mpiviennaclcuda.cu
    )
endif ()
if (PETSC_HAVE_HYPRE)
  list (APPEND PETSCVEC_SRCS
    src/vec/vec/impls/hypre/vhyp.c
    )
endif ()
if (PETSC_USE_FORTRAN_KERNELS)
  list (APPEND PETSCVEC_SRCS
    src/vec/vec/impls/seq/ftn-kernels/fwaxpy.F
    src/vec/vec/impls/seq/ftn-kernels/faypx.F
    src/vec/vec/impls/seq/ftn-kernels/fnorm.F
    src/vec/vec/impls/seq/ftn-kernels/fxtimesy.F
    src/vec/vec/impls/seq/ftn-kernels/fmdot.F
    src/vec/vec/impls/seq/ftn-kernels/fmaxpy.F
    )
endif ()
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCVEC_SRCS
    src/vec/vscat/interface/ftn-custom/zvscatfcef.c
    src/vec/is/utils/ftn-custom/zisltogf.c
    src/vec/is/utils/ftn-custom/zvsectionisf.c
    src/vec/is/is/impls/block/ftn-custom/zblockf.c
    src/vec/is/is/interface/ftn-custom/zindexf.c
    src/vec/is/is/utils/ftn-custom/ziscoloringf.c
    src/vec/is/ao/impls/mapping/ftn-custom/zaomappingf.c
    src/vec/is/ao/impls/basic/ftn-custom/zaobasicf.c
    src/vec/is/ao/interface/ftn-custom/zaof.c
    src/vec/is/sf/interface/ftn-custom/zsf.c
    src/vec/vec/impls/seq/ftn-custom/zbvec2f.c
    src/vec/vec/impls/seq/ftn-custom/zvsectionf.c
    src/vec/vec/impls/nest/ftn-custom/zvecnestf.c
    src/vec/vec/impls/mpi/ftn-custom/zpbvecf.c
    src/vec/vec/interface/ftn-custom/zvecregf.c
    src/vec/vec/interface/ftn-custom/zvectorf.c
    )
endif ()
if (PETSC_HAVE_MATLAB_ENGINE)
  list (APPEND PETSCVEC_SRCS
    src/vec/pf/impls/matlab/cmatlab.c
    src/vec/vec/utils/matlab/gcreatev.c
    )
endif ()
list (APPEND PETSCVEC_SRCS
  src/vec/vscat/impls/vscat.c
  src/vec/vscat/impls/vpscat.c
  src/vec/vscat/impls/vpscat_mpi1.c
  src/vec/vscat/interface/vscreate.c
  src/vec/vscat/interface/dlregisvecscat.c
  src/vec/vscat/interface/vscatfce.c
  src/vec/is/utils/isio.c
  src/vec/is/utils/isltog.c
  src/vec/is/utils/pmap.c
  src/vec/is/utils/vsectionis.c
  src/vec/is/is/impls/stride/stride.c
  src/vec/is/is/impls/block/block.c
  src/vec/is/is/impls/general/general.c
  src/vec/is/is/interface/index.c
  src/vec/is/is/interface/isregall.c
  src/vec/is/is/interface/isreg.c
  src/vec/is/is/utils/iscomp.c
  src/vec/is/is/utils/iscoloring.c
  src/vec/is/is/utils/isdiff.c
  src/vec/is/is/utils/isblock.c
  src/vec/is/ao/impls/memscalable/aomemscalable.c
  src/vec/is/ao/impls/mapping/aomapping.c
  src/vec/is/ao/impls/basic/aobasic.c
  src/vec/is/ao/interface/ao.c
  src/vec/is/ao/interface/dlregisdm.c
  src/vec/is/ao/interface/aoreg.c
  src/vec/is/ao/interface/aoregall.c
  src/vec/is/sf/impls/basic/sfbasic.c
  src/vec/is/sf/interface/dlregissf.c
  src/vec/is/sf/interface/sfregi.c
  src/vec/is/sf/interface/sf.c
  src/vec/is/sf/interface/sftype.c
  src/vec/pf/impls/constant/const.c
  src/vec/pf/impls/string/cstring.c
  src/vec/pf/interface/pf.c
  src/vec/pf/interface/pfall.c
  src/vec/vec/impls/node/vecnode.c
  src/vec/vec/impls/seq/bvec2.c
  src/vec/vec/impls/seq/bvec1.c
  src/vec/vec/impls/seq/dvec2.c
  src/vec/vec/impls/seq/vseqcr.c
  src/vec/vec/impls/seq/bvec3.c
  src/vec/vec/impls/nest/vecnest.c
  src/vec/vec/impls/mpi/pbvec.c
  src/vec/vec/impls/mpi/pdvec.c
  src/vec/vec/impls/mpi/pvec2.c
  src/vec/vec/impls/mpi/vmpicr.c
  src/vec/vec/impls/mpi/commonmpvec.c
  src/vec/vec/impls/shared/shvec.c
  src/vec/vec/interface/vector.c
  src/vec/vec/interface/veccreate.c
  src/vec/vec/interface/vecreg.c
  src/vec/vec/interface/vecregall.c
  src/vec/vec/interface/dlregisvec.c
  src/vec/vec/interface/rvector.c
  src/vec/vec/utils/vinv.c
  src/vec/vec/utils/vecio.c
  src/vec/vec/utils/comb.c
  src/vec/vec/utils/vecstash.c
  src/vec/vec/utils/vecmpitoseq.c
  src/vec/vec/utils/vecs.c
  src/vec/vec/utils/vsection.c
  src/vec/vec/utils/projection.c
  src/vec/vec/utils/vecglvis.c
  src/vec/vec/utils/tagger/impls/simple.c
  src/vec/vec/utils/tagger/impls/absolute.c
  src/vec/vec/utils/tagger/impls/relative.c
  src/vec/vec/utils/tagger/impls/cdf.c
  src/vec/vec/utils/tagger/impls/andor.c
  src/vec/vec/utils/tagger/impls/or.c
  src/vec/vec/utils/tagger/impls/and.c
  src/vec/vec/utils/tagger/interface/tagger.c
  src/vec/vec/utils/tagger/interface/taggerregi.c
  src/vec/vec/utils/tagger/interface/dlregistagger.c
  )
if (PETSC_HAVE_VIENNACL_NO_CUDA AND PETSC_HAVE_VIENNACL)
  list (APPEND PETSCVEC_SRCS
    src/vec/vec/impls/seq/seqviennacl/vecviennacl.cxx
    src/vec/vec/impls/mpi/mpiviennacl/mpiviennacl.cxx
    )
endif ()
if (PETSC_HAVE_MPI_TYPE_DUP AND PETSC_HAVE_MPI_WIN_CREATE)
  list (APPEND PETSCVEC_SRCS
    src/vec/is/sf/impls/window/sfwindow.c
    )
endif ()
if (PETSC_HAVE_VECCUDA)
  list (APPEND PETSCVEC_SRCS
    src/vec/vec/impls/seq/seqcuda/veccuda.c
    src/vec/vec/impls/seq/seqcuda/veccuda2.cu
    src/vec/vec/impls/seq/seqcuda/vecscattercuda.cu
    src/vec/vec/impls/mpi/mpicuda/mpicuda.cu
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCVEC_SRCS
    src/vec/is/utils/f90-custom/zisltogf90.c
    src/vec/is/utils/f90-custom/zvsectionisf90.c
    src/vec/is/is/impls/f90-custom/zblockf90.c
    src/vec/is/is/interface/f90-custom/zindexf90.c
    src/vec/is/is/utils/f90-custom/ziscoloringf90.c
    src/vec/f90-mod/petscvecmod.F
    src/vec/vec/interface/f90-custom/zvectorf90.c
    src/vec/vec/utils/f90-custom/zvsectionf90.c
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petscvec ${PETSCVEC_SRCS})
  else ()
    add_library (petscvec ${PETSCVEC_SRCS})
  endif ()
  target_link_libraries (petscvec petscsys ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petscvec PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petscvec PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()
if (PETSC_HAVE_MKL_PARDISO)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/mkl_pardiso/mkl_pardiso.c
    src/mat/impls/aij/seq/mkl_pardiso/mkl_utils.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_PARTY)
  list (APPEND PETSCMAT_SRCS
    src/mat/partition/impls/party/ftn-custom/zpartyf.c
    )
endif ()
list (APPEND PETSCMAT_SRCS
  src/mat/color/impls/minpack/color.c
  src/mat/color/impls/minpack/degr.c
  src/mat/color/impls/minpack/dsm.c
  src/mat/color/impls/minpack/ido.c
  src/mat/color/impls/minpack/numsrt.c
  src/mat/color/impls/minpack/seq.c
  src/mat/color/impls/minpack/setr.c
  src/mat/color/impls/minpack/slo.c
  src/mat/color/impls/natural/natural.c
  src/mat/color/impls/jp/jp.c
  src/mat/color/impls/power/power.c
  src/mat/color/impls/greedy/greedy.c
  src/mat/color/interface/matcoloring.c
  src/mat/color/interface/matcoloringregi.c
  src/mat/color/utils/bipartite.c
  src/mat/color/utils/valid.c
  src/mat/color/utils/weights.c
  src/mat/partition/partition.c
  src/mat/partition/spartition.c
  src/mat/partition/impls/hierarchical/hierarchical.c
  src/mat/matfd/fdmatrix.c
  src/mat/coarsen/coarsen.c
  src/mat/coarsen/scoarsen.c
  src/mat/coarsen/impls/hem/hem.c
  src/mat/coarsen/impls/mis/mis.c
  src/mat/interface/matrix.c
  src/mat/interface/matreg.c
  src/mat/interface/matregis.c
  src/mat/interface/matnull.c
  src/mat/interface/dlregismat.c
  src/mat/impls/submat/submat.c
  src/mat/impls/is/matis.c
  src/mat/impls/fft/fft.c
  src/mat/impls/baij/seq/baij.c
  src/mat/impls/baij/seq/baij2.c
  src/mat/impls/baij/seq/baijfact.c
  src/mat/impls/baij/seq/baijfact2.c
  src/mat/impls/baij/seq/dgefa.c
  src/mat/impls/baij/seq/dgedi.c
  src/mat/impls/baij/seq/dgefa3.c
  src/mat/impls/baij/seq/dgefa4.c
  src/mat/impls/baij/seq/dgefa5.c
  src/mat/impls/baij/seq/dgefa2.c
  src/mat/impls/baij/seq/dgefa6.c
  src/mat/impls/baij/seq/dgefa7.c
  src/mat/impls/baij/seq/aijbaij.c
  src/mat/impls/baij/seq/baijfact3.c
  src/mat/impls/baij/seq/baijfact4.c
  src/mat/impls/baij/seq/baijfact5.c
  src/mat/impls/baij/seq/baijfact7.c
  src/mat/impls/baij/seq/baijfact9.c
  src/mat/impls/baij/seq/baijfact11.c
  src/mat/impls/baij/seq/baijfact13.c
  src/mat/impls/baij/seq/baijfact81.c
  src/mat/impls/baij/seq/baijsolvtrannat.c
  src/mat/impls/baij/seq/baijsolvtran.c
  src/mat/impls/baij/seq/baijsolv.c
  src/mat/impls/baij/seq/baijsolvnat.c
  src/mat/impls/baij/mpi/mpibaij.c
  src/mat/impls/baij/mpi/mmbaij.c
  src/mat/impls/baij/mpi/baijov.c
  src/mat/impls/baij/mpi/mpb_baij.c
  src/mat/impls/sell/seq/sell.c
  src/mat/impls/sell/seq/fdsell.c
  src/mat/impls/sell/mpi/mpisell.c
  src/mat/impls/sell/mpi/mmsell.c
  src/mat/impls/dense/seq/dense.c
  src/mat/impls/dense/mpi/mpidense.c
  src/mat/impls/dense/mpi/mmdense.c
  src/mat/impls/normal/normm.c
  src/mat/impls/normal/normmh.c
  src/mat/impls/mffd/mffd.c
  src/mat/impls/mffd/mffddef.c
  src/mat/impls/mffd/mfregis.c
  src/mat/impls/mffd/wp.c
  src/mat/impls/python/pythonmat.c
  src/mat/impls/adj/mpi/mpiadj.c
  src/mat/impls/maij/maij.c
  src/mat/impls/lrc/lrc.c
  src/mat/impls/shell/shell.c
  src/mat/impls/shell/shellcnv.c
  src/mat/impls/composite/mcomposite.c
  src/mat/impls/nest/matnest.c
  src/mat/impls/transpose/transm.c
  src/mat/impls/transpose/htransm.c
  src/mat/impls/localref/mlocalref.c
  src/mat/impls/dummy/matdummy.c
  src/mat/impls/blockmat/seq/blockmat.c
  src/mat/impls/sbaij/seq/sbaij.c
  src/mat/impls/sbaij/seq/sbaij2.c
  src/mat/impls/sbaij/seq/sbaijfact.c
  src/mat/impls/sbaij/seq/sbaijfact2.c
  src/mat/impls/sbaij/seq/sro.c
  src/mat/impls/sbaij/seq/sbaijfact3.c
  src/mat/impls/sbaij/seq/sbaijfact4.c
  src/mat/impls/sbaij/seq/sbaijfact5.c
  src/mat/impls/sbaij/seq/sbaijfact6.c
  src/mat/impls/sbaij/seq/sbaijfact7.c
  src/mat/impls/sbaij/seq/sbaijfact8.c
  src/mat/impls/sbaij/seq/sbaijfact9.c
  src/mat/impls/sbaij/seq/sbaijfact10.c
  src/mat/impls/sbaij/seq/sbaijfact11.c
  src/mat/impls/sbaij/seq/sbaijfact12.c
  src/mat/impls/sbaij/seq/aijsbaij.c
  src/mat/impls/sbaij/mpi/mpisbaij.c
  src/mat/impls/sbaij/mpi/mmsbaij.c
  src/mat/impls/sbaij/mpi/sbaijov.c
  src/mat/impls/sbaij/mpi/mpiaijsbaij.c
  src/mat/impls/aij/seq/aij.c
  src/mat/impls/aij/seq/aijfact.c
  src/mat/impls/aij/seq/ij.c
  src/mat/impls/aij/seq/fdaij.c
  src/mat/impls/aij/seq/matmatmult.c
  src/mat/impls/aij/seq/symtranspose.c
  src/mat/impls/aij/seq/matptap.c
  src/mat/impls/aij/seq/matrart.c
  src/mat/impls/aij/seq/inode.c
  src/mat/impls/aij/seq/inode2.c
  src/mat/impls/aij/seq/matmatmatmult.c
  src/mat/impls/aij/seq/mattransposematmult.c
  src/mat/impls/aij/seq/bas/basfactor.c
  src/mat/impls/aij/seq/bas/spbas.c
  src/mat/impls/aij/seq/aijsell/aijsell.c
  src/mat/impls/aij/seq/aijperm/aijperm.c
  src/mat/impls/aij/seq/crl/crl.c
  src/mat/impls/aij/mpi/mpiaij.c
  src/mat/impls/aij/mpi/mmaij.c
  src/mat/impls/aij/mpi/mpiaijpc.c
  src/mat/impls/aij/mpi/mpiov.c
  src/mat/impls/aij/mpi/fdmpiaij.c
  src/mat/impls/aij/mpi/mpiptap.c
  src/mat/impls/aij/mpi/mpimatmatmult.c
  src/mat/impls/aij/mpi/mpb_aij.c
  src/mat/impls/aij/mpi/mpimatmatmatmult.c
  src/mat/impls/aij/mpi/mpimattransposematmult.c
  src/mat/impls/aij/mpi/aijsell/mpiaijsell.c
  src/mat/impls/aij/mpi/aijperm/mpiaijperm.c
  src/mat/impls/aij/mpi/crl/mcrl.c
  src/mat/impls/scatter/mscatter.c
  src/mat/impls/preallocator/matpreallocator.c
  src/mat/order/sp1wd.c
  src/mat/order/spnd.c
  src/mat/order/spqmd.c
  src/mat/order/sprcm.c
  src/mat/order/sorder.c
  src/mat/order/spectral.c
  src/mat/order/sregis.c
  src/mat/order/degree.c
  src/mat/order/fnroot.c
  src/mat/order/genqmd.c
  src/mat/order/qmdqt.c
  src/mat/order/rcm.c
  src/mat/order/fn1wd.c
  src/mat/order/gen1wd.c
  src/mat/order/genrcm.c
  src/mat/order/qmdrch.c
  src/mat/order/rootls.c
  src/mat/order/fndsep.c
  src/mat/order/gennd.c
  src/mat/order/qmdmrg.c
  src/mat/order/qmdupd.c
  src/mat/order/wbm.c
  src/mat/utils/convert.c
  src/mat/utils/matstash.c
  src/mat/utils/axpy.c
  src/mat/utils/zerodiag.c
  src/mat/utils/factorschur.c
  src/mat/utils/getcolv.c
  src/mat/utils/gcreate.c
  src/mat/utils/freespace.c
  src/mat/utils/compressedrow.c
  src/mat/utils/multequal.c
  src/mat/utils/matstashspace.c
  src/mat/utils/pheap.c
  src/mat/utils/bandwidth.c
  src/mat/utils/overlapsplit.c
  src/mat/utils/zerorows.c
  )
if (PETSC_HAVE_ESSL AND PETSC_USE_REAL_DOUBLE AND NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/essl/essl.c
    )
endif ()
if (PETSC_HAVE_PARMETIS)
  list (APPEND PETSCMAT_SRCS
    src/mat/partition/impls/pmetis/pmetis.c
    )
endif ()
if (PETSC_HAVE_HYPRE)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/hypre/mhypre.c
    )
endif ()
if (PETSC_HAVE_SUITESPARSE)
  list (APPEND PETSCMAT_SRCS
    src/mat/order/amd/amd.c
    )
endif ()
if (PETSC_HAVE_MKL_SPARSE)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/baij/seq/baijmkl/baijmkl.c
    src/mat/impls/baij/mpi/baijmkl/mpibaijmkl.c
    src/mat/impls/aij/seq/aijmkl/aijmkl.c
    src/mat/impls/aij/mpi/aijmkl/mpiaijmkl.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_FFTW)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/fft/fftw/ftn-custom/zfftwf.c
    )
endif ()
if (PETSC_HAVE_MATLAB_ENGINE)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/matlab/aijmatlab.c
    )
endif ()
if (PETSC_HAVE_FFTW)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/fft/fftw/fftw.c
    )
endif ()
if (PETSC_HAVE_CHACO)
  list (APPEND PETSCMAT_SRCS
    src/mat/partition/impls/chaco/chaco.c
    )
endif ()
if (PETSC_HAVE_SUITESPARSE AND PETSC_USE_REAL_DOUBLE)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/sbaij/seq/cholmod/sbaijcholmod.c
    src/mat/impls/aij/seq/umfpack/umfpack.c
    src/mat/impls/aij/seq/klu/klu.c
    src/mat/impls/aij/seq/cholmod/aijcholmod.c
    )
endif ()
if (PETSC_HAVE_LUSOL AND PETSC_USE_REAL_DOUBLE AND NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/lusol/lusol.c
    )
endif ()
if (PETSC_HAVE_VIENNACL_NO_CUDA AND PETSC_HAVE_VIENNACL)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/seqviennacl/aijviennacl.cxx
    src/mat/impls/aij/mpi/mpiviennacl/mpiaijviennacl.cxx
    )
endif ()
if (PETSC_HAVE_SUPERLU_DIST)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/mpi/superlu_dist/superlu_dist.c
    )
endif ()
if (PETSC_HAVE_CUDA AND PETSC_HAVE_VIENNACL)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/seqviennaclcuda/aijviennaclcuda.cu
    src/mat/impls/aij/mpi/mpiviennaclcuda/mpiaijviennaclcuda.cu
    )
endif ()
if (PETSC_USE_FORTRAN_KERNELS)
  list (APPEND PETSCMAT_SRCS
    src/mat/ftn-kernels/sgemv.F
    src/mat/impls/baij/seq/ftn-kernels/fsolvebaij.F
    src/mat/impls/aij/seq/ftn-kernels/fmult.F
    src/mat/impls/aij/seq/ftn-kernels/fmultadd.F
    src/mat/impls/aij/seq/ftn-kernels/fsolve.F
    src/mat/impls/aij/seq/ftn-kernels/frelax.F
    src/mat/impls/aij/seq/crl/ftn-kernels/fmultcrl.F
    )
endif ()
if (PETSC_HAVE_PASTIX)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/mpi/pastix/pastix.c
    )
endif ()
if (PETSC_HAVE_PTSCOTCH)
  list (APPEND PETSCMAT_SRCS
    src/mat/partition/impls/scotch/scotch.c
    )
endif ()
if (PETSC_HAVE_PARTY)
  list (APPEND PETSCMAT_SRCS
    src/mat/partition/impls/party/party.c
    )
endif ()
if (PETSC_HAVE_VECCUDA)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/seqcusparse/aijcusparse.cu
    src/mat/impls/aij/mpi/mpicusparse/mpiaijcusparse.cu
    )
endif ()
if (PETSC_HAVE_CUDA AND PETSC_USE_REAL_SINGLE AND PETSC_USE_COMPLEX)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/cufft/cufft.cu
    )
endif ()
if (PETSC_HAVE_MKL_CPARDISO)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/mpi/mkl_cpardiso/mkl_cpardiso.c
    )
endif ()
if (PETSC_HAVE_ELEMENTAL)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/elemental/matelem.cxx
    src/mat/impls/aij/mpi/clique/clique.cxx
    )
endif ()
if (PETSC_HAVE_STRUMPACK)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/mpi/strumpack/strumpack.c
    )
endif ()
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCMAT_SRCS
    src/mat/ftn-custom/zmat.c
    src/mat/color/interface/ftn-custom/zmatcoloringf.c
    src/mat/partition/ftn-custom/zpartitionf.c
    src/mat/matfd/ftn-custom/zfdmatrixf.c
    src/mat/interface/ftn-custom/zmatregf.c
    src/mat/interface/ftn-custom/zmatrixf.c
    src/mat/interface/ftn-custom/zmatnullf.c
    src/mat/impls/fft/ftn-custom/zfftf.c
    src/mat/impls/baij/seq/ftn-custom/zbaijf.c
    src/mat/impls/baij/mpi/ftn-custom/zmpibaijf.c
    src/mat/impls/sell/seq/ftn-custom/zsellf.c
    src/mat/impls/dense/seq/ftn-custom/zdensef.c
    src/mat/impls/dense/mpi/ftn-custom/zmpidensef.c
    src/mat/impls/mffd/ftn-custom/zmffdf.c
    src/mat/impls/python/ftn-custom/zpythonmf.c
    src/mat/impls/adj/mpi/ftn-custom/zmpiadjf.c
    src/mat/impls/shell/ftn-custom/zshellf.c
    src/mat/impls/nest/ftn-custom/zmatnestf.c
    src/mat/impls/sbaij/seq/ftn-custom/zsbaijf.c
    src/mat/impls/sbaij/mpi/ftn-custom/zmpisbaijf.c
    src/mat/impls/aij/seq/ftn-custom/zaijf.c
    src/mat/impls/aij/mpi/ftn-custom/zmpiaijf.c
    src/mat/order/ftn-custom/zsorderf.c
    )
endif ()
if (PETSC_HAVE_MUMPS)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/mpi/mumps/mumps.c
    )
endif ()
if (PETSC_HAVE_SUPERLU)
  list (APPEND PETSCMAT_SRCS
    src/mat/impls/aij/seq/superlu/superlu.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCMAT_SRCS
    src/mat/interface/f90-custom/zmatrixf90.c
    src/mat/f90-mod/petscmatmod.F
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petscmat ${PETSCMAT_SRCS})
  else ()
    add_library (petscmat ${PETSCMAT_SRCS})
  endif ()
  target_link_libraries (petscmat petscvec petscsys ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petscmat PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petscmat PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()
if (PETSC_HAVE_FFTW AND PETSC_USE_REAL_DOUBLE)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/da/usfft/matusfft.c
    )
endif ()
if (PETSC_HAVE_OPENCL)
  list (APPEND PETSCDM_SRCS
    src/dm/dt/fe/impls/opencl/feopencl.c
    )
endif ()
if (PETSC_HAVE_HYPRE)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/da/hypre/mhyp.c
    )
endif ()
if (PETSC_HAVE_TRIANGLE)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/plex/generators/triangle/trigenerate.c
    )
endif ()
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCDM_SRCS
    src/dm/label/ftn-custom/zdmlabel.c
    src/dm/impls/shell/ftn-custom/zdmshellf.c
    src/dm/impls/composite/ftn-custom/zfddaf.c
    src/dm/impls/da/ftn-custom/zdaf.c
    src/dm/impls/da/ftn-custom/zda1f.c
    src/dm/impls/da/ftn-custom/zda2f.c
    src/dm/impls/da/ftn-custom/zda3f.c
    src/dm/impls/da/ftn-custom/zdaghostf.c
    src/dm/impls/da/ftn-custom/zdacornf.c
    src/dm/impls/da/ftn-custom/zdagetscatterf.c
    src/dm/impls/da/ftn-custom/zdaviewf.c
    src/dm/impls/da/ftn-custom/zdaindexf.c
    src/dm/impls/da/ftn-custom/zdasubf.c
    src/dm/impls/plex/ftn-custom/zplex.c
    src/dm/impls/plex/ftn-custom/zplexcreate.c
    src/dm/impls/plex/ftn-custom/zplexdistribute.c
    src/dm/impls/plex/ftn-custom/zplexinterpolate.c
    src/dm/impls/plex/ftn-custom/zplexsubmesh.c
    src/dm/impls/plex/ftn-custom/zplexexodusii.c
    src/dm/impls/plex/ftn-custom/zplexgmsh.c
    src/dm/impls/plex/ftn-custom/zplexfluent.c
    src/dm/dt/interface/ftn-custom/zdtf.c
    src/dm/dt/interface/ftn-custom/zdtfef.c
    src/dm/interface/ftn-custom/zdmf.c
    src/dm/interface/ftn-custom/zdmgetf.c
    )
endif ()
list (APPEND PETSCDM_SRCS
  src/dm/label/dmlabel.c
  src/dm/field/impls/shell/dmfieldshell.c
  src/dm/field/impls/ds/dmfieldds.c
  src/dm/field/impls/da/dmfieldda.c
  src/dm/field/interface/dmfield.c
  src/dm/field/interface/dmfieldregi.c
  src/dm/field/interface/dlregisdmfield.c
  src/dm/impls/shell/dmshell.c
  src/dm/impls/network/networkcreate.c
  src/dm/impls/network/network.c
  src/dm/impls/network/networkmonitor.c
  src/dm/impls/composite/pack.c
  src/dm/impls/composite/packm.c
  src/dm/impls/redundant/dmredundant.c
  src/dm/impls/patch/patchcreate.c
  src/dm/impls/patch/patch.c
  src/dm/impls/sliced/sliced.c
  src/dm/impls/swarm/swarm.c
  src/dm/impls/swarm/data_bucket.c
  src/dm/impls/swarm/data_ex.c
  src/dm/impls/swarm/swarm_migrate.c
  src/dm/impls/swarm/swarmpic.c
  src/dm/impls/swarm/swarmpic_da.c
  src/dm/impls/swarm/swarmpic_plex.c
  src/dm/impls/swarm/swarmpic_view.c
  src/dm/impls/swarm/swarmpic_sort.c
  src/dm/impls/forest/forest.c
  src/dm/impls/da/da2.c
  src/dm/impls/da/da1.c
  src/dm/impls/da/da3.c
  src/dm/impls/da/daghost.c
  src/dm/impls/da/dacorn.c
  src/dm/impls/da/dagtol.c
  src/dm/impls/da/daltol.c
  src/dm/impls/da/daindex.c
  src/dm/impls/da/dascatter.c
  src/dm/impls/da/dacreate.c
  src/dm/impls/da/dadestroy.c
  src/dm/impls/da/dalocal.c
  src/dm/impls/da/dadist.c
  src/dm/impls/da/daview.c
  src/dm/impls/da/dasub.c
  src/dm/impls/da/gr1.c
  src/dm/impls/da/gr2.c
  src/dm/impls/da/dagtona.c
  src/dm/impls/da/dainterp.c
  src/dm/impls/da/dapf.c
  src/dm/impls/da/dagetarray.c
  src/dm/impls/da/dagetelem.c
  src/dm/impls/da/da.c
  src/dm/impls/da/dareg.c
  src/dm/impls/da/fdda.c
  src/dm/impls/da/grvtk.c
  src/dm/impls/da/dageometry.c
  src/dm/impls/da/dadd.c
  src/dm/impls/da/dapreallocate.c
  src/dm/impls/da/grglvis.c
  src/dm/impls/plex/plexcreate.c
  src/dm/impls/plex/plex.c
  src/dm/impls/plex/plexpartition.c
  src/dm/impls/plex/plexdistribute.c
  src/dm/impls/plex/plexrefine.c
  src/dm/impls/plex/plexadapt.c
  src/dm/impls/plex/plexcoarsen.c
  src/dm/impls/plex/plexinterpolate.c
  src/dm/impls/plex/plexpreallocate.c
  src/dm/impls/plex/plexreorder.c
  src/dm/impls/plex/plexgeometry.c
  src/dm/impls/plex/plexsubmesh.c
  src/dm/impls/plex/plexhdf5.c
  src/dm/impls/plex/plexhdf5xdmf.c
  src/dm/impls/plex/plexexodusii.c
  src/dm/impls/plex/plexgmsh.c
  src/dm/impls/plex/plexfluent.c
  src/dm/impls/plex/plexcgns.c
  src/dm/impls/plex/plexmed.c
  src/dm/impls/plex/plexply.c
  src/dm/impls/plex/plexvtk.c
  src/dm/impls/plex/plexpoint.c
  src/dm/impls/plex/plexvtu.c
  src/dm/impls/plex/plexfem.c
  src/dm/impls/plex/plexfvm.c
  src/dm/impls/plex/plexindices.c
  src/dm/impls/plex/plextree.c
  src/dm/impls/plex/plexgenerate.c
  src/dm/impls/plex/plexorient.c
  src/dm/impls/plex/plexnatural.c
  src/dm/impls/plex/plexproject.c
  src/dm/impls/plex/plexglvis.c
  src/dm/impls/plex/glexg.c
  src/dm/impls/plex/petscpartmatpart.c
  src/dm/dt/interface/dt.c
  src/dm/dt/interface/dtfv.c
  src/dm/dt/interface/dtds.c
  src/dm/dt/dualspace/impls/simple/dspacesimple.c
  src/dm/dt/dualspace/impls/lagrange/dspacelagrange.c
  src/dm/dt/dualspace/interface/dualspace.c
  src/dm/dt/space/impls/subspace/spacesubspace.c
  src/dm/dt/space/impls/tensor/spacetensor.c
  src/dm/dt/space/impls/poly/spacepoly.c
  src/dm/dt/space/impls/point/spacepoint.c
  src/dm/dt/space/interface/space.c
  src/dm/dt/fe/impls/composite/fecomposite.c
  src/dm/dt/fe/impls/basic/febasic.c
  src/dm/dt/fe/interface/fe.c
  src/dm/dt/fe/interface/fegeom.c
  src/dm/interface/dm.c
  src/dm/interface/dmregall.c
  src/dm/interface/dmget.c
  src/dm/interface/dmi.c
  src/dm/interface/dlregisdmdm.c
  )
if (PETSC_HAVE_CTETGEN)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/plex/generators/ctetgen/ctetgenerate.c
    )
endif ()
if (PETSC_HAVE_MOAB)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/moab/dmmoab.cxx
    src/dm/impls/moab/dmmbvec.cxx
    src/dm/impls/moab/dmmbmat.cxx
    src/dm/impls/moab/dmmbfield.cxx
    src/dm/impls/moab/dmmbmg.cxx
    src/dm/impls/moab/dmmbfem.cxx
    src/dm/impls/moab/dmmbio.cxx
    src/dm/impls/moab/dmmbutil.cxx
    )
endif ()
if (PETSC_HAVE_TETGEN)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/plex/generators/tetgen/tetgenerate.cxx
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/composite/f90-custom/zfddaf90.c
    src/dm/impls/da/f90-custom/zda1f90.c
    src/dm/impls/plex/f90-custom/zplexf90.c
    src/dm/impls/plex/f90-custom/zplexgeometryf90.c
    src/dm/dt/interface/f90-custom/zdtf90.c
    src/dm/dt/interface/f90-custom/zdtdsf90.c
    src/dm/f90-mod/petscdmmod.F
    src/dm/f90-mod/petscdmplexmod.F
    )
endif ()
if (PETSC_HAVE_P4EST)
  list (APPEND PETSCDM_SRCS
    src/dm/impls/forest/p4est/dmp4est.c
    src/dm/impls/forest/p4est/dmp8est.c
    src/dm/impls/forest/p4est/petsc_p4est_package.c
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petscdm ${PETSCDM_SRCS})
  else ()
    add_library (petscdm ${PETSCDM_SRCS})
  endif ()
  target_link_libraries (petscdm petscmat petscvec petscsys ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petscdm PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petscdm PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()
if (PETSC_HAVE_SPAI)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/spai/ispai.c
    src/ksp/pc/impls/spai/dspai.c
    )
endif ()
if (PETSC_HAVE_ML)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/ml/ml.c
    )
endif ()
if (NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/tfs/bitmask.c
    src/ksp/pc/impls/tfs/comm.c
    src/ksp/pc/impls/tfs/gs.c
    src/ksp/pc/impls/tfs/ivec.c
    src/ksp/pc/impls/tfs/xxt.c
    src/ksp/pc/impls/tfs/xyt.c
    src/ksp/pc/impls/tfs/tfs.c
    src/ksp/ksp/impls/gmres/dgmres/dgmres.c
    src/ksp/ksp/impls/gmres/agmres/agmres.c
    src/ksp/ksp/impls/gmres/agmres/agmresorthog.c
    src/ksp/ksp/impls/gmres/agmres/agmresleja.c
    src/ksp/ksp/impls/gmres/agmres/agmresdeflation.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_HYPRE)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/hypre/ftn-custom/zhypref.c
    )
endif ()
if (PETSC_HAVE_SAWS)
  list (APPEND PETSCKSP_SRCS
    src/ksp/ksp/interface/saws/kspsaws.c
    )
endif ()
if (PETSC_HAVE_CUDA AND PETSC_HAVE_VIENNACL)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/chowiluviennaclcuda/chowiluviennacl.cu
    src/ksp/pc/impls/saviennaclcuda/saviennacl.cu
    src/ksp/pc/impls/rowscalingviennaclcuda/rowscalingviennacl.cu
    )
endif ()
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/bjacobi/ftn-custom/zbjacobif.c
    src/ksp/pc/impls/python/ftn-custom/zpythonpcf.c
    src/ksp/pc/impls/factor/ftn-custom/zluf.c
    src/ksp/pc/impls/shell/ftn-custom/zshellpcf.c
    src/ksp/pc/impls/gasm/ftn-custom/zgasmf.c
    src/ksp/pc/impls/composite/ftn-custom/zcompositef.c
    src/ksp/pc/impls/gamg/ftn-custom/zgamgf.c
    src/ksp/pc/impls/fieldsplit/ftn-custom/zfieldsplitf.c
    src/ksp/pc/impls/asm/ftn-custom/zasmf.c
    src/ksp/pc/impls/mg/ftn-custom/zmgf.c
    src/ksp/pc/impls/mg/ftn-custom/zmgfuncf.c
    src/ksp/pc/interface/ftn-custom/zpcsetf.c
    src/ksp/pc/interface/ftn-custom/zpreconf.c
    src/ksp/ksp/impls/python/ftn-custom/zpythonkspf.c
    src/ksp/ksp/impls/gmres/fgmres/ftn-custom/zmodpcff.c
    src/ksp/ksp/interface/ftn-custom/zitclf.c
    src/ksp/ksp/interface/ftn-custom/zitcreatef.c
    src/ksp/ksp/interface/ftn-custom/zitfuncf.c
    src/ksp/ksp/interface/ftn-custom/zxonf.c
    src/ksp/ksp/interface/ftn-custom/zdmkspf.c
    src/ksp/ksp/interface/ftn-custom/ziguess.c
    )
endif ()
if (PETSC_HAVE_HYPRE)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/hypre/hypre.c
    )
endif ()
list (APPEND PETSCKSP_SRCS
  src/ksp/pc/impls/bddc/bddc.c
  src/ksp/pc/impls/bddc/bddcprivate.c
  src/ksp/pc/impls/bddc/bddcgraph.c
  src/ksp/pc/impls/bddc/bddcscalingbasic.c
  src/ksp/pc/impls/bddc/bddcnullspace.c
  src/ksp/pc/impls/bddc/bddcfetidp.c
  src/ksp/pc/impls/bddc/bddcschurs.c
  src/ksp/pc/impls/lsc/lsc.c
  src/ksp/pc/impls/bjacobi/bjacobi.c
  src/ksp/pc/impls/is/pcis.c
  src/ksp/pc/impls/is/nn/nn.c
  src/ksp/pc/impls/sor/sor.c
  src/ksp/pc/impls/pbjacobi/pbjacobi.c
  src/ksp/pc/impls/cp/cp.c
  src/ksp/pc/impls/eisens/eisen.c
  src/ksp/pc/impls/telescope/telescope.c
  src/ksp/pc/impls/telescope/telescope_dmda.c
  src/ksp/pc/impls/ksp/pcksp.c
  src/ksp/pc/impls/python/pythonpc.c
  src/ksp/pc/impls/vpbjacobi/vpbjacobi.c
  src/ksp/pc/impls/factor/factor.c
  src/ksp/pc/impls/factor/factimpl.c
  src/ksp/pc/impls/factor/icc/icc.c
  src/ksp/pc/impls/factor/lu/lu.c
  src/ksp/pc/impls/factor/ilu/ilu.c
  src/ksp/pc/impls/factor/cholesky/cholesky.c
  src/ksp/pc/impls/jacobi/jacobi.c
  src/ksp/pc/impls/kaczmarz/kaczmarz.c
  src/ksp/pc/impls/galerkin/galerkin.c
  src/ksp/pc/impls/wb/wb.c
  src/ksp/pc/impls/shell/shellpc.c
  src/ksp/pc/impls/gasm/gasm.c
  src/ksp/pc/impls/mat/pcmat.c
  src/ksp/pc/impls/composite/composite.c
  src/ksp/pc/impls/none/none.c
  src/ksp/pc/impls/lmvm/lmvmpc.c
  src/ksp/pc/impls/gamg/gamg.c
  src/ksp/pc/impls/gamg/agg.c
  src/ksp/pc/impls/gamg/geo.c
  src/ksp/pc/impls/gamg/util.c
  src/ksp/pc/impls/gamg/classical.c
  src/ksp/pc/impls/fieldsplit/fieldsplit.c
  src/ksp/pc/impls/asm/asm.c
  src/ksp/pc/impls/mg/mg.c
  src/ksp/pc/impls/mg/fmg.c
  src/ksp/pc/impls/mg/smg.c
  src/ksp/pc/impls/mg/mgfunc.c
  src/ksp/pc/impls/redistribute/redistribute.c
  src/ksp/pc/impls/svd/svd.c
  src/ksp/pc/impls/redundant/redundant.c
  src/ksp/pc/impls/patch/pcpatch.c
  src/ksp/pc/interface/precon.c
  src/ksp/pc/interface/pcset.c
  src/ksp/pc/interface/pcregis.c
  src/ksp/ksp/impls/minres/minres.c
  src/ksp/ksp/impls/bicg/bicg.c
  src/ksp/ksp/impls/fcg/fcg.c
  src/ksp/ksp/impls/fcg/pipefcg/pipefcg.c
  src/ksp/ksp/impls/cg/cg.c
  src/ksp/ksp/impls/cg/cgeig.c
  src/ksp/ksp/impls/cg/cgtype.c
  src/ksp/ksp/impls/cg/cgls.c
  src/ksp/ksp/impls/cg/gltr/gltr.c
  src/ksp/ksp/impls/cg/cgne/cgne.c
  src/ksp/ksp/impls/cg/pipecgrr/pipecgrr.c
  src/ksp/ksp/impls/cg/stcg/stcg.c
  src/ksp/ksp/impls/cg/groppcg/groppcg.c
  src/ksp/ksp/impls/cg/pipelcg/pipelcg.c
  src/ksp/ksp/impls/cg/nash/nash.c
  src/ksp/ksp/impls/cg/pipecg/pipecg.c
  src/ksp/ksp/impls/cr/cr.c
  src/ksp/ksp/impls/cr/pipecr/pipecr.c
  src/ksp/ksp/impls/fetidp/fetidp.c
  src/ksp/ksp/impls/ibcgs/ibcgs.c
  src/ksp/ksp/impls/python/pythonksp.c
  src/ksp/ksp/impls/tsirm/tsirm.c
  src/ksp/ksp/impls/gcr/gcr.c
  src/ksp/ksp/impls/gcr/pipegcr/pipegcr.c
  src/ksp/ksp/impls/qcg/qcg.c
  src/ksp/ksp/impls/rich/rich.c
  src/ksp/ksp/impls/rich/richscale.c
  src/ksp/ksp/impls/bcgsl/bcgsl.c
  src/ksp/ksp/impls/lsqr/lsqr.c
  src/ksp/ksp/impls/cheby/cheby.c
  src/ksp/ksp/impls/bcgs/bcgs.c
  src/ksp/ksp/impls/bcgs/pipebcgs/pipebcgs.c
  src/ksp/ksp/impls/bcgs/fbcgs/fbcgs.c
  src/ksp/ksp/impls/bcgs/fbcgsr/fbcgsr.c
  src/ksp/ksp/impls/lcd/lcd.c
  src/ksp/ksp/impls/preonly/preonly.c
  src/ksp/ksp/impls/gmres/gmres.c
  src/ksp/ksp/impls/gmres/borthog.c
  src/ksp/ksp/impls/gmres/borthog2.c
  src/ksp/ksp/impls/gmres/gmres2.c
  src/ksp/ksp/impls/gmres/gmreig.c
  src/ksp/ksp/impls/gmres/gmpre.c
  src/ksp/ksp/impls/gmres/pgmres/pgmres.c
  src/ksp/ksp/impls/gmres/pipefgmres/pipefgmres.c
  src/ksp/ksp/impls/gmres/lgmres/lgmres.c
  src/ksp/ksp/impls/gmres/fgmres/fgmres.c
  src/ksp/ksp/impls/gmres/fgmres/modpcf.c
  src/ksp/ksp/impls/cgs/cgs.c
  src/ksp/ksp/impls/symmlq/symmlq.c
  src/ksp/ksp/impls/tfqmr/tfqmr.c
  src/ksp/ksp/impls/tcqmr/tcqmr.c
  src/ksp/ksp/interface/itcl.c
  src/ksp/ksp/interface/itfunc.c
  src/ksp/ksp/interface/iguess.c
  src/ksp/ksp/interface/itcreate.c
  src/ksp/ksp/interface/iterativ.c
  src/ksp/ksp/interface/itres.c
  src/ksp/ksp/interface/itregis.c
  src/ksp/ksp/interface/xmon.c
  src/ksp/ksp/interface/eige.c
  src/ksp/ksp/interface/dlregisksp.c
  src/ksp/ksp/interface/dmksp.c
  src/ksp/ksp/guess/impls/pod/pod.c
  src/ksp/ksp/guess/impls/fischer/fischer.c
  src/ksp/ksp/utils/kspmatregi.c
  src/ksp/ksp/utils/dmproject.c
  src/ksp/ksp/utils/schurm/schurm.c
  src/ksp/ksp/utils/lmvm/lmvmimpl.c
  src/ksp/ksp/utils/lmvm/lmvmutils.c
  src/ksp/ksp/utils/lmvm/bfgs/bfgs.c
  src/ksp/ksp/utils/lmvm/brdn/brdn.c
  src/ksp/ksp/utils/lmvm/dfp/dfp.c
  src/ksp/ksp/utils/lmvm/symbrdn/symbrdn.c
  src/ksp/ksp/utils/lmvm/symbrdn/symbadbrdn.c
  src/ksp/ksp/utils/lmvm/sr1/sr1.c
  src/ksp/ksp/utils/lmvm/badbrdn/badbrdn.c
  src/ksp/ksp/utils/lmvm/diagbrdn/diagbrdn.c
  )
if (PETSC_HAVE_PARMS)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/parms/parms.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCKSP_SRCS
    src/ksp/f90-mod/petsckspmod.F
    src/ksp/ksp/interface/f90-custom/zitfuncf90.c
    )
endif ()
if (PETSC_HAVE_ATTRIBUTEALIGNED)
  list (APPEND PETSCKSP_SRCS
    
    )
endif ()
if (PETSC_HAVE_VIENNACL_NO_CUDA AND PETSC_HAVE_VIENNACL)
  list (APPEND PETSCKSP_SRCS
    src/ksp/pc/impls/saviennacl/saviennacl.cxx
    src/ksp/pc/impls/rowscalingviennacl/rowscalingviennacl.cxx
    src/ksp/pc/impls/chowiluviennacl/chowiluviennacl.cxx
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petscksp ${PETSCKSP_SRCS})
  else ()
    add_library (petscksp ${PETSCKSP_SRCS})
  endif ()
  target_link_libraries (petscksp petscdm petscmat petscvec petscsys ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petscksp PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petscksp PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()
if (PETSC_HAVE_SAWS)
  list (APPEND PETSCSNES_SRCS
    src/snes/interface/saws/snessaws.c
    )
endif ()
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCSNES_SRCS
    src/snes/utils/ftn-custom/zdmdasnesf.c
    src/snes/utils/ftn-custom/zdmlocalsnesf.c
    src/snes/utils/ftn-custom/zdmsnesf.c
    src/snes/interface/ftn-custom/zsnesf.c
    src/snes/impls/shell/ftn-custom/zsnesshellf.c
    src/snes/impls/python/ftn-custom/zpythonsf.c
    src/snes/linesearch/impls/shell/ftn-custom/zlinesearchshellf.c
    src/snes/linesearch/interface/ftn-custom/zlinesearchf.c
    )
endif ()
list (APPEND PETSCSNES_SRCS
  src/snes/mf/snesmfj.c
  src/snes/utils/dmsnes.c
  src/snes/utils/dmdasnes.c
  src/snes/utils/dmlocalsnes.c
  src/snes/utils/dmplexsnes.c
  src/snes/utils/convest.c
  src/snes/utils/dmadapt.c
  src/snes/interface/snes.c
  src/snes/interface/snesj.c
  src/snes/interface/snesregi.c
  src/snes/interface/snesut.c
  src/snes/interface/snesj2.c
  src/snes/interface/dlregissnes.c
  src/snes/interface/snesob.c
  src/snes/interface/snespc.c
  src/snes/impls/vi/vi.c
  src/snes/impls/vi/ss/viss.c
  src/snes/impls/vi/rs/virs.c
  src/snes/impls/shell/snesshell.c
  src/snes/impls/gs/snesgs.c
  src/snes/impls/gs/gssecant.c
  src/snes/impls/ngmres/snesngmres.c
  src/snes/impls/ngmres/ngmresfunc.c
  src/snes/impls/ngmres/anderson.c
  src/snes/impls/composite/snescomposite.c
  src/snes/impls/fas/fas.c
  src/snes/impls/fas/fasgalerkin.c
  src/snes/impls/fas/fasfunc.c
  src/snes/impls/ksponly/ksponly.c
  src/snes/impls/tr/tr.c
  src/snes/impls/python/pythonsnes.c
  src/snes/impls/ls/ls.c
  src/snes/impls/ms/ms.c
  src/snes/impls/qn/qn.c
  src/snes/impls/richardson/snesrichardson.c
  src/snes/impls/ncg/snesncg.c
  src/snes/impls/nasm/nasm.c
  src/snes/impls/nasm/aspin.c
  src/snes/linesearch/impls/shell/linesearchshell.c
  src/snes/linesearch/impls/bt/linesearchbt.c
  src/snes/linesearch/impls/l2/linesearchl2.c
  src/snes/linesearch/impls/basic/linesearchbasic.c
  src/snes/linesearch/impls/cp/linesearchcp.c
  src/snes/linesearch/impls/nleqerr/linesearchnleqerr.c
  src/snes/linesearch/interface/linesearch.c
  src/snes/linesearch/interface/linesearchregi.c
  )
if (PETSC_USE_REAL_DOUBLE AND NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCSNES_SRCS
    src/snes/interface/noise/snesmfj2.c
    src/snes/interface/noise/snesnoise.c
    src/snes/interface/noise/snesdnest.c
    )
endif ()
if (PETSC_HAVE_ATTRIBUTEALIGNED)
  list (APPEND PETSCSNES_SRCS
    
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCSNES_SRCS
    src/snes/utils/f90-custom/zdmplexsnesf90.c
    src/snes/interface/f90-custom/zsnesf90.c
    src/snes/f90-mod/petscsnesmod.F
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petscsnes ${PETSCSNES_SRCS})
  else ()
    add_library (petscsnes ${PETSCSNES_SRCS})
  endif ()
  target_link_libraries (petscsnes petscksp petscdm petscmat petscvec petscsys ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petscsnes PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petscsnes PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()
if (NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCTS_SRCS
    src/ts/characteristic/impls/da/slda.c
    src/ts/characteristic/interface/characteristic.c
    src/ts/characteristic/interface/mocregis.c
    src/ts/characteristic/interface/slregis.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_HAVE_SUNDIALS)
  list (APPEND PETSCTS_SRCS
    src/ts/impls/implicit/sundials/ftn-custom/zsundialsf.c
    )
endif ()
if (PETSC_HAVE_SUNDIALS)
  list (APPEND PETSCTS_SRCS
    src/ts/impls/implicit/sundials/sundials.c
    )
endif ()
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCTS_SRCS
    src/ts/interface/ftn-custom/ztscreatef.c
    src/ts/interface/ftn-custom/ztsf.c
    src/ts/interface/ftn-custom/ztsregf.c
    src/ts/trajectory/interface/ftn-custom/ztrajf.c
    src/ts/impls/python/ftn-custom/zpythontf.c
    src/ts/impls/explicit/ssp/ftn-custom/zsspf.c
    src/ts/impls/explicit/rk/ftn-custom/zrkf.c
    src/ts/impls/arkimex/ftn-custom/zarkimexf.c
    src/ts/impls/rosw/ftn-custom/zroswf.c
    src/ts/adapt/impls/dsp/ftn-custom/zadaptdspf.c
    src/ts/adapt/interface/ftn-custom/ztsadaptf.c
    )
endif ()
list (APPEND PETSCTS_SRCS
  src/ts/utils/dmts.c
  src/ts/utils/dmlocalts.c
  src/ts/utils/dmdats.c
  src/ts/utils/dmplexts.c
  src/ts/event/tsevent.c
  src/ts/interface/ts.c
  src/ts/interface/tscreate.c
  src/ts/interface/tsreg.c
  src/ts/interface/tsregall.c
  src/ts/interface/dlregists.c
  src/ts/interface/tseig.c
  src/ts/interface/tsrhssplit.c
  src/ts/interface/sensitivity/tssen.c
  src/ts/trajectory/impls/visualization/trajvisualization.c
  src/ts/trajectory/impls/memory/trajmemory.c
  src/ts/trajectory/impls/singlefile/singlefile.c
  src/ts/trajectory/impls/basic/trajbasic.c
  src/ts/trajectory/interface/traj.c
  src/ts/impls/bdf/bdf.c
  src/ts/impls/python/pythonts.c
  src/ts/impls/pseudo/posindep.c
  src/ts/impls/glee/glee.c
  src/ts/impls/explicit/ssp/ssp.c
  src/ts/impls/explicit/euler/euler.c
  src/ts/impls/explicit/rk/rk.c
  src/ts/impls/mimex/mimex.c
  src/ts/impls/arkimex/arkimex.c
  src/ts/impls/eimex/eimex.c
  src/ts/impls/rosw/rosw.c
  src/ts/impls/implicit/alpha/alpha1.c
  src/ts/impls/implicit/alpha/alpha2.c
  src/ts/impls/implicit/glle/glle.c
  src/ts/impls/implicit/glle/glleadapt.c
  src/ts/impls/implicit/theta/theta.c
  src/ts/impls/symplectic/basicsymplectic/basicsymplectic.c
  src/ts/adapt/impls/cfl/adaptcfl.c
  src/ts/adapt/impls/glee/adaptglee.c
  src/ts/adapt/impls/none/adaptnone.c
  src/ts/adapt/impls/dsp/adaptdsp.c
  src/ts/adapt/impls/basic/adaptbasic.c
  src/ts/adapt/interface/tsadapt.c
  )
if (PETSC_HAVE_RADAU5)
  list (APPEND PETSCTS_SRCS
    src/ts/impls/implicit/radau5/radau5.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCTS_SRCS
    src/ts/f90-mod/petsctsmod.F
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petscts ${PETSCTS_SRCS})
  else ()
    add_library (petscts ${PETSCTS_SRCS})
  endif ()
  target_link_libraries (petscts petscsnes petscksp petscdm petscmat petscvec petscsys ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petscts PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petscts PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()
if (NOT PETSC_USE_COMPLEX)
  list (APPEND PETSCTAO_SRCS
    src/tao/bound/impls/blmvm/blmvm.c
    src/tao/bound/impls/bqnk/bqnk.c
    src/tao/bound/impls/bqnk/bqnkls.c
    src/tao/bound/impls/bqnk/bqnktr.c
    src/tao/bound/impls/bqnk/bqnktl.c
    src/tao/bound/impls/bnk/bnk.c
    src/tao/bound/impls/bnk/bnls.c
    src/tao/bound/impls/bnk/bntr.c
    src/tao/bound/impls/bnk/bntl.c
    src/tao/bound/impls/bqnls/bqnls.c
    src/tao/bound/impls/bncg/bncg.c
    src/tao/bound/impls/tron/tron.c
    src/tao/bound/utils/isutil.c
    src/tao/pde_constrained/impls/lcl/lcl.c
    src/tao/complementarity/impls/ssls/ssls.c
    src/tao/complementarity/impls/ssls/ssils.c
    src/tao/complementarity/impls/ssls/ssfls.c
    src/tao/complementarity/impls/asls/asils.c
    src/tao/complementarity/impls/asls/asfls.c
    src/tao/quadratic/impls/gpcg/gpcg.c
    src/tao/quadratic/impls/bqpip/bqpip.c
    src/tao/unconstrained/impls/nls/nls.c
    src/tao/unconstrained/impls/neldermead/neldermead.c
    src/tao/unconstrained/impls/ntr/ntr.c
    src/tao/unconstrained/impls/cg/taocg.c
    src/tao/unconstrained/impls/lmvm/lmvm.c
    src/tao/unconstrained/impls/bmrm/bmrm.c
    src/tao/unconstrained/impls/ntl/ntl.c
    src/tao/unconstrained/impls/owlqn/owlqn.c
    src/tao/constrained/impls/ipm/ipm.c
    src/tao/linesearch/impls/armijo/armijo.c
    src/tao/linesearch/impls/morethuente/morethuente.c
    src/tao/linesearch/impls/owarmijo/owarmijo.c
    src/tao/linesearch/impls/unit/unit.c
    src/tao/linesearch/impls/gpcglinesearch/gpcglinesearch.c
    src/tao/leastsquares/impls/pounders/pounders.c
    src/tao/leastsquares/impls/pounders/gqt.c
    )
endif ()
if (PETSC_HAVE_FORTRAN AND PETSC_USING_F90)
  list (APPEND PETSCTAO_SRCS
    src/tao/f90-mod/petsctaomod.F
    )
endif ()
list (APPEND PETSCTAO_SRCS
  src/tao/matrix/adamat.c
  src/tao/matrix/submatfree.c
  src/tao/util/tao_util.c
  src/tao/interface/taosolver.c
  src/tao/interface/taosolver_fg.c
  src/tao/interface/taosolverregi.c
  src/tao/interface/taosolver_hj.c
  src/tao/interface/taosolver_bounds.c
  src/tao/interface/dlregistao.c
  src/tao/interface/fdiff.c
  src/tao/linesearch/interface/taolinesearch.c
  src/tao/linesearch/interface/dlregis_taolinesearch.c
  )
if (PETSC_HAVE_FORTRAN)
  list (APPEND PETSCTAO_SRCS
    src/tao/interface/ftn-custom/ztaosolverf.c
    src/tao/linesearch/interface/ftn-custom/ztaolinesearchf.c
    )
endif ()

if (NOT PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petsctao ${PETSCTAO_SRCS})
  else ()
    add_library (petsctao ${PETSCTAO_SRCS})
  endif ()
  target_link_libraries (petsctao petscsnes petscksp petscdm petscmat petscvec petscsys ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petsctao PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petsctao PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()
endif ()

if (PETSC_USE_SINGLE_LIBRARY)
  if (PETSC_HAVE_CUDA)
    cuda_add_library (petsc ${PETSCSYS_SRCS} ${PETSCVEC_SRCS} ${PETSCMAT_SRCS} ${PETSCDM_SRCS} ${PETSCKSP_SRCS} ${PETSCSNES_SRCS} ${PETSCTS_SRCS} ${PETSCTAO_SRCS})
  else ()
    add_library (petsc ${PETSCSYS_SRCS} ${PETSCVEC_SRCS} ${PETSCMAT_SRCS} ${PETSCDM_SRCS} ${PETSCKSP_SRCS} ${PETSCSNES_SRCS} ${PETSCTS_SRCS} ${PETSCTAO_SRCS})
  endif ()
  target_link_libraries (petsc ${PETSC_PACKAGE_LIBS})
  if (PETSC_WIN32FE)
    set_target_properties (petsc PROPERTIES RULE_LAUNCH_COMPILE "${PETSC_WIN32FE}")
    set_target_properties (petsc PROPERTIES RULE_LAUNCH_LINK "${PETSC_WIN32FE}")
  endif ()

endif ()

if (PETSC_CLANGUAGE_Cxx)
  foreach (file IN LISTS PETSCSYS_SRCS
  PETSCVEC_SRCS
  PETSCMAT_SRCS
  PETSCDM_SRCS
  PETSCKSP_SRCS
  PETSCSNES_SRCS
  PETSCTS_SRCS
  PETSCTAO_SRCS)
    if (file MATCHES "^.*\\.c$")
      set_source_files_properties(${file} PROPERTIES LANGUAGE CXX)
    endif ()
  endforeach ()
endif()