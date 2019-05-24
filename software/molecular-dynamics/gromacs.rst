.. _gromacs:

Gromacs
=======

.. sidebar:: Gromacs

  :URL: http://www.gromacs.org/
  :URL: https://www.nvidia.com/en-us/data-center/gpu-accelerated-applications/gromacs/


Gromacs is a versatile package for molecular dynamics simulations, which solves the Newtonian equations of motion for systems with hundreds to millions of particles.  Although the software scales well to hundreds of cores for typical simulations, Gromacs calculations are restricted to at most a single node on the JADE service.

Job scripts
-----------

Gromacs jobs can run using 1, 4 (half a node), or 8 (full node) GPUs (please see note below regarding job performance). The following Slurm script example is written for one of the regression tests from the installation:


::

   #!/bin/bash

   #SBATCH --nodes=1
   #SBATCH --ntasks-per-node=1
   #SBATCH --cpus-per-task=5
   #SBATCH --gres=gpu:1
   #SBATCH --gres-flags=enforce-binding
   #SBATCH --time=10:00:00
   #SBATCH -J testGromacs
   #SBATCH -p small

   module purge
   module load gromacs/2018.0

   mpirun -np ${SLURM_NTASKS_PER_NODE} --bind-to socket \
          mdrun_mpi -s topol.tpr \
	  -ntomp ${SLURM_CPUS_PER_TASK} &> run-gromacs.out


The example utilises 1 GPU (--gres=gpu:1) on a JADE node. Gromacs is started with a number of MPI processes (--ntasks-per-node=1) , which must match the number of requested GPUs. Each MPI process will run 5 OMP threads (--cpus-per-task=5). The number of requested MPI processes is saved in the environment variable SLURM_NTASKS_PER_NODE, while the number of threads per process is saved in SLURM_CPUS_PER_TASK.

The request --bind-to socket is specific to OpenMPI, which was used to build Gromacs on JADE. This extra option to the OpenMPI mpirun is essential in obtaining the optimal run configuration and computational performance.

To run the same job on 4 or 8 GPUs, you need to change the values of --ntasks-per-node and --gres=gpu from 1 to 4 or 8, respectively. You also need to change the partition from small to big. While running on 4 or 8 GPUs might increase the performance of a single job, **please note** that in terms of aggregate simulation time, it is more efficient to run single-GPU jobs. For example, a 4-GPU simulation of a 240,000 atom system yields about 18.5 ns/day using Gromacs2016.3, while a single-GPU simulation yields 8.4 ns/day. Thus, running four single-GPU simulations will provide almost double the throughput (4x8.4 ns/day = 33.6 ns/day) compared to a single 4-GPU simulation. More importantly, the single-GPU performance has been vastly improved in Gromacs2018, and is about double that of Gromacs2016.3. Thus, a single-GPU simulation using Gromacs2018 gives about the same performance as a 4-GPU simulation, and it is therefore **strongly advised to only run single-GPU simulations with Gromacs2018.**


To read more about Gromacs processing on GPUs, please visit https://www.nvidia.com/en-us/data-center/gpu-accelerated-applications/gromacs/ .

*Caution*: Run performance can be negatively affected by a deviation from the above "recipe".  Please modify `#SBATCH` parameters or `mpirun` command line options only if you are sure this is necessary.  A job that runs sub-optimally on half a node can affect the performance of another job on the other half of the same node, which would normally run at optimal performance on a "quiet" system.


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
