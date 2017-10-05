.. _getting-started:

Using the JADE Facility
=======================

If you have not used a High Performance Computing (HPC) cluster, the Linux operating system or even a command line before this is the place to start. This guide will get you set up using the JADE cluster fairly quickly.

The whole system from the user perspective is interacted with via the Slurm Workload Manager on the login nodes. Via this scheduler, access to the compute nodes, can be interactive or batch. The installed application software consists of a mixture of docker container images, supplied by Nvidia, and executables built from source. Both container images and executables can use the system either interactively or in batch mode.

It is only possible to ssh onto a node which has been allocated to the user. Once the session completes the ssh access is removed. Access to the global parallel file system is from the login nodes and all compute nodes. Any data on this file system is retained after a session on the nodes completes. There is also access to local disc space on each node. Access to this file system is only possible during a Slurm session. Once the session completes the local disc data is removed.




.. toctree::
   :maxdepth: -1

   getting-account
   connecting
   scheduler
   modules
   containers
