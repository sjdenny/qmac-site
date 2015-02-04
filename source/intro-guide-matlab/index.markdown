---
layout: page
title: "Introduction to compiling and submitting Matlab code"
comments: false
sharing: true
footer: true
---

Once you have been provided with a user account to the cluster and have familiarized yourself in how to access it (see the [Quick Start](/quick-start) guide)  you may want to run some Matlab code on the cluster nodes. This page introduces you to the basics of compiling any Matlab code by working through the compilation and use of the example Matlab function `diag_rand_matrices.m` at `/share/apps/matlab/example_code/diag_rand_matrices.m`.

For this page we will follow my user "clark".

1. Modify your `.bash_profile` file to add `Matlab` to your path, with e.g.
  {% codeblock %}
PATH=/share/apps/matlab/R2014b/bin:$PATH
  {% endcodeblock %}
See the page [Installed Software](/installed-software) for more details on the versions available.
  
* Login to qmac via an SSH terminal like putty. The terminal will start with you in the root of your home directory, in this case /home/clark. Copy the example function to your home directory:
  {% codeblock %}
mkdir -p $HOME/code/example
cp /share/apps/matlab/example_code/diag_rand_matrices.m $HOME/code/example
cd $HOME/code/example
  {% endcodeblock %}
* Now start Matlab by typing "matlab". Matlab runs in the command prompt of the terminal and cannot display plots or give any GUI interface.
* Test the example function by typing
  {% codeblock %}
diag_rand_matrices ('test', 5, 10)
  {% endcodeblock %}
  An output similar to this should be displayed: 
  {% codeblock %}
>> diag_rand_matrices ('test', 5, 10)
Random matrix diagonalization ...
Sample 1
Sample 2
Sample 3
Sample 4
Sample 5
Sample 6
Sample 7
Sample 8
Sample 9
Sample 10

spec =

  -0.4085
  -0.0626
  -0.1281
  -0.2203
    0.3802
  -0.2449
    0.3540
    0.0714
  -0.2495
    0.1805

It took 0.4793
  {% endcodeblock %}
  A Matlab data file test.mat should now be present in the directory which contains the averaged spectrum.
* The Matlab command mcc (which stands for matlab c compiler) is used to compile 
  Matlab functions into standalone executables. A custom Matlab script called compile.m has been written to automate this task for our cluster environment. To compile `diag_rand_matrices.m` simply type
  {% codeblock %} 
compile diag_rand_matrices
  {% endcodeblock %} 
  This should be quick for this function but for more complex functions it can take a few minutes so be patient. In this case we get an output like: 
  {% codeblock %} 
>> compile diag_rand_matrices
Compiling: diag_rand_matrices...
(Use compile_mt to compile with multithreading support)
Warning: You have specified '-R -nojvm', which may restrict or eliminate MATLAB graphics functionality at runtime.
Compiler version: 5.2 (R2014b)
Dependency analysis by REQUIREMENTS.
Warning: Adding path "/share/apps/matlab/MPS_library" to Compiler path
instance. 
Parsing file "/home/clark/code/example/diag_rand_matrices.m"
        (Referenced from: "Compiler Command Line").
Deleting 0 temporary MEX authorization files.
Generating file "/home/clark/code/example/readme.txt".
Generating file "run_diag_rand_matrices.sh".
Tidying up output files ...
Creating shell scripts ...
Compilation finished.
  {% endcodeblock %} 
* To check the compilation is complete take a look at the contents of the directory you are in by typing "ls". You should see the following: 
  {% codeblock %}
>> ls -lh
total 112K
-rwxrw-r-- 1 clark clark  89K Oct 21 12:27 diag_rand_matrices
-rw-rw-r-- 1 clark clark  494 Oct 21 12:03 diag_rand_matrices.m
-rwxrw-r-- 1 clark clark  513 Oct 21 12:27 diag_rand_matrices.sh
-rwxrw-r-- 1 clark clark  113 Oct 21 12:27 load.sh
drwxrwxr-x 2 clark clark 4.0K Oct 21 12:27 sge_output
-rw-rw-r-- 1 clark clark  261 Oct 21 12:04 test.mat
  {% endcodeblock %}

  Along with your original m-file (i) diag_rand_matrices.m the compilation has automatically generated (ii) the executable diag_rand_matrices, (iii) a shell script diag_rand_matrices.sh for wrapping up the executable, (iv) a shell script load.sh which submits diag_rand_matrices.sh to the Sun Grid Engine (SGE) and finally a (v) a directory sge_output where the standard output (i.e. everything displayed in the terminal) for this code will be dumped. The data file test.mat from our first run in Matlab is also present.
* Exit Matlab and go back to the Unix terminal. You can now run the new executable in the shell by typing 
  {% codeblock %}
./diag_rand_matrices.sh test 5 10
  {% endcodeblock %} 
  If you are using the SSH terminal the same output should appear that we got in Matlab above but now it is generated outside Matlab by a standalone executable running on the frontend. If you are using Putty this may not work. Instead you will get no output and the code will not run. This is not a fault with the compilation but rather an issue with the X-screen. Putty only runs the executable in the shell if you are running a remote X-screen server like Exceed. We need not be concerned by these issues since we don’t want to run our code in a shell on the frontend anyhow.
* Instead of running the code in the shell we will use the shell script load.sh to load it on to the cluster for distribution to one of the compute nodes. Thus we type 
  {% codeblock %}
./load.sh new_test 5 10
  {% endcodeblock %} 
  We use a different output file name to distinguish it from our earlier run within Matlab. The script gives a qsub output which looks like this: 
  {% codeblock %}
[clark@qmac example]$ ./load.sh new_test 5 10
Loading job
Your job 3449 ("diag_rand_matrices") has been submitted
job-ID  prior   name       user         state submit/start at     queue                          slots ja-task-ID 
-----------------------------------------------------------------------------------------------------------------
   3448 0.00000 diag_rand_ clark        qw    10/21/2014 14:08:01                                    1       
  {% endcodeblock %} 


  It shows that the executable has been submitted and that it is in the state "qw" signifying that it is waiting in the queue to be distributed to the cluster.
* Check that the job changes to the running "r" state by using the qstat command a few times more. For the parameters above the job should rapidly finish and nothing no jobs will be shown by qstat.
* If you go to the sge_output directory there should now be a new file with the name diag_rand_matrices.o### where ### is the job ID number (3448 in the above example) that was allocated to this job. If you open this file with a text editor it will contain (along with a warning about there being no display) the output that we originally got when we ran this code in Matlab. If everything looks OK with the output then we can delete this file.
* Now our job submitted to the cluster also outputted a file new_test.mat. We can change where this file gets saved easily. Let’s make a data directory by typing `mkdir data` and run `./load.sh data/new_test 5 10`. This should now output a new data file new_test.mat to our newly made data directory.
* Congratulations you have ran your first piece of Matlab code on the cluster. Continue to the page Notes on Matlab compiling for more details and tips of how to compile your own more complex Matlab code.
