.. _software:

Software on JADE
================

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
    :hidden:
    :maxdepth: 2
    :glob:

    apps/index
    libs/index
    development/index
