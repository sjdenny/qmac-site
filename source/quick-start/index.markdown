---
layout: page
title: "quick start"
comments: true
sharing: true
footer: true
---

Accounts
---------
Contact [Samuel Denny](mailto:s.denny@physics.ox.ac.uk) or [Stephen Clark](mailto:s.clark@physics.ox.ac.uk) to acquire an account.

Login and access
------------------

The cluster is configured such that the only machine you interact with directly is qmac.physics.ox.ac.uk. 
It stores your data/programs, and dispatches your jobs to compute nodes in an efficient manner.

Access to qmac.physics.ox.ac.uk is via SSH. 
On Linux, run
{% codeblock %}
ssh username@qmac.physics.ox.ac.uk
{% endcodeblock %}
to login.
On Windows, the [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) program will enable you to connect.

To transfer files between the QMAC cluster and your work machine, an SFTP client is generally easiest. 
[FileZilla](https://filezilla-project.org) is a popular choice. 
Additionally, under Linux, you may use the sshfs utility to mount the cluster filesystem and thus access it in the same manner as you would a local drive. 
You would perform the mount with
{% codeblock %}
sshfs username@qmac.physics.ox.ac.uk:/home/username/ /home/username/qmac
{% endcodeblock %}
to mount the filesystem at /home/username/qmac  (this must have been previously created.)


Submitting jobs
----------------

Follow either of the following. 

* [Python example](/scientific-computing-with-python)
* [Stephen's Matlab example](/intro-guide-matlab) (includes details on compiling Matlab code for the cluster)
