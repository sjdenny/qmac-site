---
layout: page
title: "Scientific Computing with Python"
comments: true
sharing: true
footer: true
---

## TODO:
* Add something on using iPython Notebook effectively.

## Useful resources 
* [Software Carpentry](http://software-carpentry.org/v5/novice/python/index.html) has a good introduction.
* The book [Python for Data Analysis](http://www.amazon.co.uk/Python-Data-Analysis-Wrangling-IPython/dp/1449319793) is excellent for covering common operations and workflow when getting started with Python.
* [Python for Matlab users](http://mathesaurus.sourceforge.net/matlab-numpy.html)
* [Matplotlib](http://matplotlib.org/) is a powerful plotting package, enabling publication-quality plots to be created from datasets with no postprocessing required. They have a [tutorial here](http://matplotlib.org/users/pyplot_tutorial.html).


## Using Python on the QMAC cluster ##
In this section, we will set up python on a user account, and use it to run a job of "diagonalise random matrices", in parallel with the [Matlab guide](intro-guide-matlab).

### Setting up your user account ###
See [here](installed-software) for the details of the various python installations available. 
There is a system installation of python at `/usr/bin/python`: generally we will not use this, since components of the cluster software rely on this installation remaining static and not being upgraded.
There are installations of python optimised for speed in `/share/apps/python`, and you will need to add some matter to your `.bash_profile` file in order to make use of these.

Picking a specific installation, we add the following to our `.bash_profile`:
{% codeblock %}
source /share/apps/intel/2013.5.192/bin/compilervars.sh intel64
export PATH=/share/apps/python/2.7.8/3/bin:$PATH
source /share/apps/python/2.7.8/3/bin/virtualenvwrapper.sh
{% endcodeblock %}

Log out and in for the changes to take effect, and we find our default python has been set:
{% codeblock %}
[denny@qmac ~]$ which python
/share/apps/python/2.7.8/3/bin/python
{% endcodeblock %}

In general, it is good to use virtual environments to manage python installations. You can read more [here](http://docs.python-guide.org/en/latest/dev/virtualenvs/). For now, the following will suffice. 
Create a new virtual environment called "diagRandMatrices" with
{% codeblock %}
[denny@qmac ~]$ mkvirtualenv diagRandMatrices
New python executable in diagRandMatrices/bin/python
Installing setuptools, pip...done.

(diagRandMatrices)[denny@qmac ~]$ 
{% endcodeblock %}
The prefix `(diagRandMatrices)` indicates that this virtualenv is activated. `deactivate` will switch back to our default python, and `workon diagRandMatrices` will reactivate the virtual environment. By default, system libraries are not imported when we create a virtualenv. We can enable them with
{% codeblock %}
(diagRandMatrices)[denny@qmac ~]$ toggleglobalsitepackages 
Enabled global site-packages
{% endcodeblock %}

which gives us access to the minimal set of packages installed in `/share/apps/python/`, in particular `numpy` and `scipy` which have been built with the Intel compilers and linked the the MKL library.

Let's check that this is working:
{% codeblock %}
(diagRandMatrices)[denny@qmac ~]$ python -c "import numpy; print numpy.version"
<module 'numpy.version' from '/share/apps/python/2.7.8/3/lib/python2.7/site-packages/numpy/version.pyc'>
{% endcodeblock %}

### Basic Python script ###

Have a look at the file `/share/apps/python/example_code/diag_rand_matrices.py`
{% codeblock %}
import numpy as np
import scipy.io
import time

def diag_rand_matrices(savename,N,samples):

    print 'Random matrix diagonalisation ...'

    start_time = time.time()
    spectrum = np.zeros([samples, N],dtype=complex)
    for j in range(0,samples):
        # Generate matrix:
        mat = np.random.uniform(low=-1, high=1, size=(N,N))
        evals, evecs = np.linalg.eig(mat)
        spectrum[j,:] = evals

    mean_spectrum = np.mean(spectrum, axis=1)
    vars_to_save = {'mean_spectrum': mean_spectrum}
    scipy.io.savemat(savename, vars_to_save, appendmat=False)

    end_time = time.time()
    print 'It took {duration:.2f} seconds.'.format(duration = (end_time-start_time))
{% endcodeblock %}

This is the Python analogue of the Matlab tutorial script described [here](/intro-guide-matlab). It defines a function `diag_rand_matrices`, which generates `samples` `N x N` matrices, diagonalises them, and saves the average of their spectra to a Matlab file specified by `savename`. Being Python, we don't need to compile this prior to running it.

We can run this function interactively as follows. Start `ipython`, and type
{% codeblock %}
%run /share/apps/python/example_code/diag_rand_matrices.py
{% endcodeblock %}
This will execute the code in `diag_rand_matrices.py` in your ipython session, defining the function diag_rand_matrices(...). We can call it as 

{% codeblock %}
(diagRandMatrices)[denny@qmac ~]$ ipython
WARNING: Attempting to work in a virtualenv. If you encounter problems, please install IPython inside the virtualenv.
Python 2.7.8 (default, Nov  8 2014, 14:27:00) 
Type "copyright", "credits" or "license" for more information.

IPython 2.3.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: %run /share/apps/python/example_code/diag_rand_matrices.py

In [2]: diag_rand_matrices('spectrum.mat',100,10)
Random matrix diagonalisation ...
It took 0.59 seconds.
{% endcodeblock %}

### Running on the cluster ###

Typically we pass in an argument to set the specific parameters for a job within a batch. We need to add some code to handle this.

### TODO: ####

* Submit to the job scheduler as a single job
* Submit a batch, with reasonable job management (avoiding collision of outputs, etc)