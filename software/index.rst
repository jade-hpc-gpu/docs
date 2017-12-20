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
    OpenMPI,   2.1.2, Inlcudes multi-thread support
    Gromacs,	2016.3,	Supplied by Nvidia
    NAMD,	2.12,

This software has been built from source and installed as modules. To list the source built applications do:

::

    $ module avail
    ----------------------------------------------------- /jmain01/apps/modules -----------------------------
    gromacs/2016.3           openmpi/1.10.2/2017      pgi/17.4(default)        pgi64/17.4(default)
    PrgEnv-pgi/17.4(default)      NAMD/2.12      openmpi/1.10.5a1/GNU     pgi/2017             pgi64/2017



The following are the available applications, libraries and development tools on the JADE cluster:

.. toctree::
    :maxdepth: 2
    :glob:

    machine-learning/index
    molecular-dynamics/index
    python
