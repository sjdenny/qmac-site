---
layout: page
title: "Available Software"
comments: true
sharing: true
footer: true
---

As multiple versions of pieces of software may be available, it is necessary for you to add the relevant path to your .bash_profile file. 
This involves adding a line such as
{% codeblock %}
PATH=/share/apps/matlab/R2014b/bin:$PATH
{% endcodeblock %}

to your .bash_profile in your /home/username/ directory.

Matlab
-------------
*Note, in some versions of Matlab, and on certain platforms, there is an intermittent bug regarding the convergence of the SVD routine. 
To make a preliminary check that the version you are using is not affected, ensure the following Matlab code runs successfully. 
If you receive an error message that the SVD has not converged, use a different version of Matlab.*
{% codeblock %}
>> load /share/apps/matlab/example_code/error.mat
>> whos
  Name       Size            Bytes  Class     Attributes

  C         48x36            27648  double    complex   

>> [U,S,V] = svd(C);
{% endcodeblock %}


* version R2014b  
  Add /share/apps/matlab/R2014b/bin to .bash_profile:
  {% codeblock %}
PATH=/share/apps/matlab/R2014b/bin:$PATH
  {% endcodeblock %}
  Toolboxes currently available:

  * Matlab compiler
  * Statistics toolbox
  
  (Note, adding many more toolboxes slows down the Matlab compiler substantially, unless these are then removed from the compiler search path.)
  

* Additional versions may be added as they become available. Older versions are also available if required. 
    
Mathematica
-----------
Not currently installed, but can be made available if there is any demand. 
Note that Mathematica has a "gridMathematica" component which is aimed at parallelisation across many CPUs/GPUs.

Intel C and Fortran compilers, and MKL library
----------------------
#### Intel(R) Cluster Tools 2013 SP1 Update 1 ####
This includes:

* Intel(R) Fortran Compiler XE 14.0 Update 2
* Intel(R) C++ Compiler XE 14.0 Update 2
* Intel(R) Math Kernel Library 11.1 Update 2
* Intel(R) Debugger 13.0
* Intel(R) MPI Library, Development Kit 4.1 Update 3
* Intel(R) Trace Analyzer and Collector 8.1 Update 4                          
* Intel(R) Integrated Performance Primitives 8.1
* Intel(R) Threading Building Blocks 4.2 Update 3
    
Add the following to your .bash_profile, to appropriately modify your PATH and LD_LIBRARY_PATH environment variables.
{% codeblock %}
source /share/apps/intel/2013.1.046/bin/compilervars.sh intel64
{% endcodeblock %}

The path for the MKL libraries is /share/apps/intel/2013.1.046/mkl 

#### Intel(R) Composer XE 2013 Update 5 ####
This includes:

* Intel(R) Fortran Compiler XE 13.1 Update 3
* Intel(R) C++ Compiler XE 13.1 Update 3
* Intel(R) Math Kernel Library 11.0 Update 5
* Others...

Add the following to your .bash_profile, to appropriately modify your PATH and LD_LIBRARY_PATH environment variables.
{% codeblock %}
source /share/apps/intel/2013.5.192/bin/compilervars.sh intel64
{% endcodeblock %}


Python, numpy, scipy, pandas, Intel MKL
-----------------------------------------

A number of central installations are provided, differing in the versions of python, numpy, scipy provided, as well as the compilers with which they were built. 
Python is built with gcc. NumPy/SciPy are typically built with the Intel compilers for maximum speed, and linked to Intel MKL for fast BLAS routines.

Of the two installations presently available, version 1 was built with a more recent version of the Intel compilers and is linked to a more recent MKL library, however has a handful of (unimportant?) SciPy tests failing. 
Version 2 uses older Intel software, but has no significant unit test fails.

#### Installation 1 ####
This includes:

* Python 2.7.8
* NumPy 1.8.2
* SciPy 0.14.0

NumPy and SciPy are built with `icc 14.0.2`, and linked to `MKL 11.1.2`.
To use, add the following to your .bash_profile:
{% codeblock %}
source /share/apps/intel/2013.1.046/bin/compilervars.sh intel64
export PATH=/share/apps/python/2.7.8/2/bin:$PATH
source /share/apps/python/2.7.8/2/bin/virtualenvwrapper.sh
{% endcodeblock %}

Verify the numerics libraries are working as expected:
{% codeblock %}
[denny@qmac ~]$ python -c "import numpy; numpy.test()"
Running unit tests for numpy
NumPy version 1.8.2
NumPy is installed in /share/apps/python/2.7.8/2/lib/python2.7/site-packages/numpy
Python version 2.7.8 (default, Nov  8 2014, 13:35:58) [GCC 4.4.7 20120313 (Red Hat 4.4.7-11)]
nose version 1.3.4
[ ... ]
----------------------------------------------------------------------
Ran 5313 tests in 64.979s

OK (KNOWNFAIL=5, SKIP=3)


[denny@qmac ~]$ python -c "import scipy; scipy.test()"
Running unit tests for scipy
NumPy version 1.8.2
NumPy is installed in /share/apps/python/2.7.8/2/lib/python2.7/site-packages/numpy
SciPy version 0.14.0
SciPy is installed in /share/apps/python/2.7.8/2/lib/python2.7/site-packages/scipy
Python version 2.7.8 (default, Nov  8 2014, 13:35:58) [GCC 4.4.7 20120313 (Red Hat 4.4.7-11)]
nose version 1.3.4
[ ... ]
----------------------------------------------------------------------
Ran 16579 tests in 233.845s

FAILED (KNOWNFAIL=278, SKIP=1178, errors=1, failures=3)
{% endcodeblock %}

* The failed test `test_fitpack.TestSplder.test_kink` is a common issue, and can probably be ignored. See [here](https://github.com/scipy/scipy/issues/2911) for more details.
* The three failed tests (test_lorentz, test_multi, test_pearson) appear to result from optimisations made by the `ifort` compiler, [see here](https://github.com/scipy/scipy/issues/1205). If you require the `odr` SciPy module, you should use the other installation.



#### Installation 2 ####
This includes:

* Python 2.7.8
* NumPy 1.8.2
* SciPy 0.14.0

NumPy and SciPy are built with `icc 13.1.3`, and linked to `MKL 11.0.5`.
To use, add the following to your .bash_profile:
{% codeblock %}
source /share/apps/intel/2013.5.192/bin/compilervars.sh intel64
export PATH=/share/apps/python/2.7.8/3/bin:$PATH
source /share/apps/python/2.7.8/3/bin/virtualenvwrapper.sh
{% endcodeblock %}

Verify the numerics libraries are working as expected:
{% codeblock %}
[denny@qmac ~]$ python -c "import numpy; numpy.test()"
Running unit tests for numpy
NumPy version 1.8.2
NumPy is installed in /share/apps/python/2.7.8/3/lib/python2.7/site-packages/numpy
Python version 2.7.8 (default, Nov  8 2014, 14:27:00) [GCC 4.4.7 20120313 (Red Hat 4.4.7-11)]
nose version 1.3.4
[ ... ]
----------------------------------------------------------------------
Ran 5313 tests in 68.865s

OK (KNOWNFAIL=5, SKIP=3)


[denny@qmac ~]$ python -c "import scipy; scipy.test()"
Running unit tests for scipy
NumPy version 1.8.2
NumPy is installed in /share/apps/python/2.7.8/3/lib/python2.7/site-packages/numpy
SciPy version 0.14.0
SciPy is installed in /share/apps/python/2.7.8/3/lib/python2.7/site-packages/scipy
Python version 2.7.8 (default, Nov  8 2014, 14:27:00) [GCC 4.4.7 20120313 (Red Hat 4.4.7-11)]
nose version 1.3.4
[ ... ]
----------------------------------------------------------------------
Ran 16579 tests in 241.927s

FAILED (KNOWNFAIL=278, SKIP=1178, errors=1)
{% endcodeblock %}
The failed test `test_fitpack.TestSplder.test_kink` is a common issue, and can probably be ignored. See [here](https://github.com/scipy/scipy/issues/2911) for more details.


TNT library
----------------------
See [here](/tnt-library).


NAG libraries
------------------------
* NAG C library Mark 24: /share/apps/nag/cllux24dcl
* NAG Fortran library Mark 24: /share/apps/nag/fll6i24dcl

Add the following to your .bash_profile:
{% codeblock %}
export NAG_KUSARI_FILE=/share/apps/nag/licence.lic
{% endcodeblock %}
