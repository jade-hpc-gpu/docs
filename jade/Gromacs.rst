.. _gromacs

Gromacs
=======

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

The following is an example Slurm script to run the code using one of the regression tests from the installation:

::

    #!/bin/bash
    #SBATCH --nodes=2
    #SBATCH --ntasks=4
    #SBATCH -p all
    #SBATCH -J Gromacs
    #SBATCH --time=01:00:00
    #SBATCH --gres=gpu:2
    module load gromacs/2016.3
    ./gmxtest.pl -np 8 -verbose simple

As can be seem the code will run across multiple compute nodes. Also, the number of GPUs per node is requested using the ``gres`` option. In this example the user will request 2 out of the possible 8 on the nodes.

There is access to the local disc space on each node whilst the batch session is in progress. It can be accessed via: ``/raid/local_scratch/$USERID``

Any data within the directory will be lost once the session completes.
