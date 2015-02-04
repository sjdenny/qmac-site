---
layout: page
title: "SGE Configuration"
comments: true
sharing: true
footer: true
---

An overview of how the job scheduler SGE is configured to match jobs to available compute nodes.

Job Scheduling
---------------

We continue to use Open Grid Scheduler (a fork of SGE), in spite of it being no longer under active development. 
You should feel free to put as many jobs on as you need, the purpose of the scheduler is to balance available nodes between users requesting jobs. 

Job scheduling is carried out on a "fill-up" basis, with one node being loaded with jobs until it is at capacity, then another, and so on. If there are jobs already on the cluster, new jobs will fill up partially full nodes before starting on idle ones. This is to encourage single-slot jobs to "group together", so that there are complete nodes free for parallel, multi-slot jobs.


Job queues
-------------
As with bose.physics.ox.ac.uk, we use a set of queues to differentiate between types of job. 
Long jobs are slot-limited (to enable other users arriving later to find some free cores left), while short jobs may fill the entire cluster (since they will by definition finish soon, and other users submitting jobs will then be prioritised by the scheduler as slots become available). 
The queues currently available are

* **all.q**: default queue, no time limit, limited to 600 jobs per user,
* **medium.q**: 24 hours (wall) time limit, limited to 450 jobs per user,
* **short.q**: 2 hours (wall) time limit, unlimited jobs.

These may be requested as 'resources' within Grid Engine, namely by use of the '-l short', '-l medium' options to qsub. 


Memory limits
--------------
Currently not implemented. Bear in mind that each compute node has 4GB memory per core, so common jobs will not typically require adjustment.

Parallel jobs
----------------
### OpenMP ###

Jobs utilising OpenMP run on a single compute node, with multiple computational threads communicating via the OpenMP framework. This is set up at job submission time by passing the argument `-pe threads N` to `qsub`, to request N slots on a single compute node. See [Debugging job submission](/debugging-job-submission) to choose an appropriate value of N.

### MPI ###

MPI jobs can span multiple compute nodes, and use the internal network for interprocess communication. Not currently available, but will be configured as needed.