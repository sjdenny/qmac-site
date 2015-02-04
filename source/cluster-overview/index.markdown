---
layout: page
title: "cluster overview"
comments: true
sharing: true
footer: true
---

Hardware
---------

The QMAC Cluster consists of 48 Dell PowerEdge C6220 compute nodes, each providing two 8-core Intel Xeon E5-2650v2 2.6GHz processors, for a total of **768 cores** and **16.0 TFLOPS**. 
Each node is fitted with 64 GB of RAM, for **4 GB per core**. 
These are connected together with a high speed 10Gb network.

The frontend, qmac.physics.ox.ac.uk, has two quad-core Intel Xeon E5-2609v2 2.5GHz processors, 32 GB of RAM, and a RAID array providing 6.5 TB of storage for user directories.

Additionally, there is a GPU node at qmac-gpu.physics.ox.ac.uk, having two Intel Xeon E5-2640v2 2.0GHz processors, 64 GB of RAM, and a Nvidia Tesla K20 GPGPU card (2496 CUDA cores, 5 GB GDDR5 memory, 208 GB/sec memory bandwidth). 

The frontend and compute nodes share a filesystem over NFS, which means your /home/ directory is mounted on the compute nodes, as well as the /share/apps/ directory for installed programs. 
