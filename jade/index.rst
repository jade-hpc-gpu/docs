.. _getting-started:

Using the JADE Facility
***********************

The whole system from the user perspective is interacted with via the Slurm Workload Manager on the login nodes. Via this scheduler, access to the compute nodes, can be interactive or batch. The installed application software consists of a mixture of docker container images, supplied by Nvidia, and executables built from source. Both container images and executables can use the system either interactively or in batch mode.

It is only possible to ssh onto a node which has been allocated to the user. Once the session completes the ssh access is removed. Access to the global parallel file system is from the login nodes and all compute nodes. Any data on this file system is retained after a session on the nodes completes. There is also access to local disc space on each node. Access to this file system is only possible during a Slurm session. Once the session completes the local disc data is removed.

The software initially installed on the machine is listed in the following table:

.. csv-table:: 
   :header: Application,Version,Note
   :widths: 20, 20, 10

    GNU compiler suite,	4.8.4,	part of O/S
    PGI compiler suite,	17.4,	 
    OpenMPI,	1.10.2,	Supplied with PGI
    OpenMPI,	1.10.5a1,	Supplied with PGI
    Gromacs,	2016.3,	Supplied by Nvidia
    NAMD,	2.12,

This software has been built from source and installed as modules. To list the source built applications do:

::

    $ module avail
    ----------------------------------------------------- /jmain01/apps/modules -----------------------------
    gromacs/2016.3           openmpi/1.10.2/2017      pgi/17.4(default)        pgi64/17.4(default)     
    PrgEnv-pgi/17.4(default)      NAMD/2.12      openmpi/1.10.5a1/GNU     pgi/2017             pgi64/2017

The applications initially supplied by Nvidia as containers are listed in the following table:

.. csv-table:: 
   :header: Application,Version
   :widths: 20, 20

    Caffe,	17.04
    Theano,	17.04
    Torch,	17.04

To list the containers and version available on the system do:

::

    $ containers
    REPOSITORY                    TAG                 IMAGE ID                CREATED               SIZE
    nvidia/cuda                  latest              15e5dedd88c5        4 weeks ago          1.67 GB
    nvcr.io/nvidia/caffe         17.04               87c288427f2d        6 weeks ago          2.794 GB
    nvcr.io/nvidia/theano        17.04               24943feafc9b         8 weeks ago          2.386 GB
    nvcr.io/nvidia/torch         17.04               a337ffb42c8e         9 weeks ago          2.9 GB

The following brief notes explain how to run the various applications.


.. toctree::
   :maxdepth: -1
   :hidden:

   Modules
   Slurm
   Gromacs
   NAMD
   containers
