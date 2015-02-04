---
layout: page
title: "Debugging job submission"
comments: true
sharing: true
footer: true
---

General notes on how to make your jobs use the compute nodes efficiently.

* Array tasks for many similar jobs (faster submission)
* Memory limits (for determining whether they need setting, how to choose optimally)
* Time limits (can get more slots)
* Parallel environment (determine optimal number of slots to specify for -pe threads, while good numbers are 4, 8, 16)