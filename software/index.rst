.. _software:

Software on JADE
================

The software initially installed on the machine is listed in the following table:

.. csv-table::
   :header: Application,Version,Note
   :widths: 20, 20, 10

    GNU compiler suite,	4.8.5,	part of O/S
    Intel compiler suite, 2021.3,
    OpenMPI,	2.1.6,
    OpenMPI,   4.0.5, Inlcudes multi-thread support
    Gromacs,	2016.3,	Supplied by Nvidia
    NAMD,	2.14,

This software has been built from source and installed as modules. To list the source built applications do:

::

   $ module avail
   ----------------------------------------------- /jmain02/apps/Modules/modulefiles/packages ------------------------------------------------------------------------------
   autodock-gpu/1.5.2   gnuplot/5.2.8                 gromacs/2021.1       namd/2.14            opencv/4.2.0   pytorch/1.9.0     tmux/3.2a (D)
   emacs/27.1           gromacs/2020.3                gromacs/2021.2 (D)   namd/3.0-alpha7 (D)  openmm/7.5.0   sox/14.4.2        vim/8.2
   ffmpeg/4.2.2         gromacs/2020.4-plumed-2.6.2   mdtraj/1.9.5         nano/5.3             plumed/2.6.2   tensorflow/2.3.1
   


The following are the available applications, libraries and development tools on the JADE cluster:

.. toctree::
    :maxdepth: 2
    :glob:

    machine-learning/index
    molecular-dynamics/index
    python
    git
    datasets
    
