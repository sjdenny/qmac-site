---
layout: page
title: "Installation instructions"
date: 2015-02-05 16:21
comments: true
sharing: true
footer: true
---

Contains installation instructions for various pieces of software installed on QMAC.

ARPACK
--------

 - Using 'new generation' ARPACK from http://forge.scilab.org/index.php/p/arpack-ng/downloads/
 - Unpack tar.gz from here (I used arpack-ng_3.1.5.tar.gz)
 - Make sure that the Intel compilers are set up: `source /share/apps/intel/2013.1.046/bin/compilervars.sh intel64`
 - set environment variables as follows:

{% codeblock %}
export F77=ifort 
export CC=icc
export MKLROOT=/share/apps/intel/2013.1.046
export CFLAGS=-openmp
export FFLAGS=-openmp
export LIBS="-L$MKLROOT/compiler/lib/intel64 -liomp5"
export BLAS_LIBS="-L$MKLROOT/mkl/lib/intel64 -lmkl_intel_lp64 -lmkl_core -lmkl_intel_thread -lpthread -lm"
export LAPACK_LIBS="-L$MKLROOT/lib/intel64 -lmkl_intel_lp64 -lmkl_core -lmkl_intel_thread -lpthread -lm"
{% endcodeblock %}

  - run `./configure --prefix=/share/apps/arpack`
  - run `make`
  - run `make install`

Libraries have been installed in: `/share/apps/arpack/lib`

If you ever happen to want to link against installed libraries
in a given directory, `LIBDIR`, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR`
flag during linking and do at least one of the following:

   - add `LIBDIR` to the `LD_LIBRARY_PATH` environment variable
     during execution
   - add `LIBDIR` to the `LD_RUN_PATH` environment variable
     during linking
   - use the `-Wl,-rpath -Wl,LIBDIR` linker flag
   - have your system administrator add LIBDIR to `/etc/ld.so.conf`

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
