.. _gromacs:

Gromacs
=======

.. sidebar:: Gromacs

  :URL: http://www.gromacs.org/
  :URL: https://www.nvidia.com/en-us/data-center/gpu-accelerated-applications/gromacs/


Gromacs is a versatile package for molecular dynamics simulations, which solves the Newtonian equations of motion for systems with hundreds to millions of particles.  Although the software scales well to hundreds of cores for typical simulations, Gromacs calculations are restricted to at most a single node on the JADE service.

Job scripts
-----------

The following is an example Slurm script to run the code using one of the regression tests from the installation:

::

    #!/bin/bash

    #SBATCH --nodes=1
    #SBATCH --ntasks=4
    #SBATCH -J testGromacs
    #SBATCH --time=01:00:00
    #SBATCH --gres=gpu:4

    module load gromacs/2016.3

    mpirun -np $SLURM_NTASKS gmx_mpi mdrun -s topol.tpr -noconfout -resethway -nsteps 10000 -ntomp 10 -pin on &> run-gromacs.out

The example utilises half the resources on a JADE node, with a requests for a single node with 20 tasks and 4 GPUs.  Gromacs is started with a number of processes to match the number of GPUs.  Also, each process is multithreading, option which is set via `-ntomp`.  Process pinning is requested via `-pin on`.

To read more about Gromacs processing on GPUs, please visit https://www.nvidia.com/en-us/data-center/gpu-accelerated-applications/gromacs/ .


Installation notes
------------------

The latest version of the source was used. The following build instructions were followed: http://www.nvidia.com/object/gromacs-installation.html

The code was compiled using OpenMPI v1.10.5a1 and GCC v4.8.4 with the following command:

::

    CC=mpicc CXX=mpicxx cmake /jmain01/home/atostest/Building/gromacs-2016.3
    -DGMX_OPENMP=ON
    -DGMX_GPU=ON
    -DGPU_DEPLOYMENT_KIT_ROOT_DIR=/usr/local/cuda-8.0/targets/x86_64-linux
    -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-8.0/targets/x86_64-linux
    -DNVML_INCLUDE_DIR=/usr/local/cuda-8.0/targets/x86_64-linux/include
    -DNVML_LIBRARY=/usr/lib/nvidia-375/libnvidia-ml.so
    -DHWLOC_INCLUDE_DIRS=/usr/mpi/gcc/openmpi-1.10.5a1/include/openmpi/opal/mca/hwloc/hwloc191/hwloc/include
    -DGMX_BUILD_OWN_FFTW=ON
    -DGMX_PREFER_STATIC_LIBS=ON
    -DCMAKE_BUILD_TYPE=Release
    -DGMX_BUILD_UNITTESTS=ON
    -DCMAKE_INSTALL_PREFIX=/jmain01/home/atostest/gromacs-2016.3
