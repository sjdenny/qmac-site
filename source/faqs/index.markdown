---
layout: page
title: "FAQs"
comments: true
sharing: true
footer: true
---

This page is a work-in-progress...

#### How do I get started? ####
[See here](/quick-start).

#### I have lots of jobs waiting in the queue, but there are free nodes available? ####
(Default is long.q, but specify -l medium, -l short and you get a higher slot limit.) --- put on a page

#### My jobs wait in the queue but don't start ####
Run qstat to see your current job IDs, then run
{% codeblock %}
qstat -j <job_ID>
{% endcodeblock %}
to get additional information about the job scheduling. Also look to see whether partial output files have been created ("qstat -j #" will tell you where these are.)

- My jobs keep on getting killed.  (Are you running over the memory limit - hopefully not an issue-to-be.)
- Do you have (some piece of software) installed?  (Check the installed software page.  Also check --link-- for details on installing it yourself.
- Should I be using a parallel environment?
- Rules of thumb for how to decide on jobs.
1)  Can it use multiple cores (8 or 16) effectively?  Use PE environment.
2)  Can it run in the standard 1 slot environment?  If too much memory, either increase a bit, or put it in a suitable PE anyway.
- How many jobs for the queue?  e.g. 1000, but point towards array tasks.