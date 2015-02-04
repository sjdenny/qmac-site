---
layout: page
title: "admin"
comments: true
sharing: true
footer: true
---

A collection of how-tos for common admin tasks on the cluster.

#### Creating a new user ####

Log in as user samuel, run
{% codeblock %}
[samuel@qmac ~]$ sudo scripts/add_user.py 
{% endcodeblock %}
and follow the instructions. This will set up the user's account, and email them with a password and a link to these pages.

#### IP ranges for the internal networks ####
* Public IPs:
  * Frontend: 163.1.137.250/255.255.255.0 (interface em4) -- qmac.physics.ox.ac.uk
  * GPU compute node: 163.1.137.251/255.255.255.0 (em4) -- qmac-gpu.physics.ox.ac.uk
* Internal data network:
  * Frontend: 10.1.1.1/255.255.0.0 (em1 and em2, on channel bonding interface bond0)
  * GPU compute node: 10.1.1.3/255.255.0.0 (em1) and 10.1.1.4/255.255.0.0 (em2)
  * Compute nodes:
    * dell-0-1: 10.1.2.1
    * ...
    * dell-0-48: 10.1.2.48
* Internal management network:
  * Frontend: 172.44.1.100 (em3)
  * GPU node: 172.44.1.101 (em3)
  * IPMI ports:
    * Frontend: 172.44.1.50
    * GPU node: 172.44.1.51
    * Compute nodes:
      * dell-0-1: 172.44.1.1
      * ...
      * dell-0-48: 172.44.1.48

#### Using IPMI to remote control node power ####

General command is `ipmi 172.44.1.$NODE chassis power off/on/soft/cycle/reset`

The following behaviours have been determined empirically from our hardware, rather than from generic documentation:

* `power soft`: shutdown by asking OS, may not work if frozen.
* `power off`: same as above.
* `power cycle`: shutdown by asking OS, then power on.
* `power reset`: forcible shutdown and restart = pushing reset button on case.
* `power on`: turn on from off state, otherwise does nothing.
* `power status`: report the power status of the machine.

NB 1: rapidly issued commands to the same host may not all take effect, e.g. `power on` immediately following `power off` may leave the machine in an "off" state.

NB 2: the nodes are configured to *always* PXE boot over the network when they restart. If a Rocks installation is detected, they will generally proceed to boot this. If not, they will reinstall from the frontend.

#### Reviving a dead node ####
TODO: add indicators that node is dead:

* ganglia
* qstat
* ssh, ping, ...

It is generally possible to revive a crashed or offline node remotely. To force a complete reinstall of Rocks, run the following.
{% codeblock %}
# ignore any local installation and tell Rocks to reinstall this node.
[root@qmac ~]#    rocks set host boot dell-0-$NODE action=install 

# force a restart
[samuel@qmac ~]$  ipmi 172.44.1.$NODE chassis power reset
{% endcodeblock %}
Now wait 10-20 minutes for the node to restart. It should reappear in [ganglia](http://qmac.physics.ox.ac.uk/ganglia), and should be accessible by ssh.
Note that for nodes in the normal state, `rocks list host boot` should fix their action as `os`, to mean that they will not reinstall on a reboot.


#### Increasing a job priority ####

As a queue manager, run
{% codeblock %}
qalter -p 1023 <JOBID>
{% endcodeblock %}
to bump it to the top of the queue


#### Modifying these webpages ####
Make sure you are a member of the group `rvm`:
{% codeblock %}
[denny@qmac ~]$ groups
denny rvm
{% endcodeblock %}

Navigate to `/export/web-docs/source/`, and modify the markdown files there. 
Run `rake generate` to generate the site from the markdown content, and `deploy_webdocs` to automatically copy this across to the webserver.

Useful commands:
{% codeblock %}
rake generate && deploy_webdocs  # to push to the webserver
jekyll serve                     # to serve the current source at qmac.physics.ox.ac.uk:4000
rake new_page["Title here"]      # to create a new page, etc.
{% endcodeblock %}

See [Jekyll](http://jekyllrb.com/)  and [Octopress](http://octopress.org/) for more information. 
Common Jekyll syntax is [here](http://sourceforge.net/p/jekyllc/bugs/markdown_syntax).