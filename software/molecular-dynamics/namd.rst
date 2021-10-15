.. _namd:

NAMD
====

.. sidebar:: NAMD

   :URL: http://www.ks.uiuc.edu/Research/namd/
   :URL: https://www.nvidia.com/en-us/data-center/gpu-accelerated-applications/namd/


NAMD is a parallel molecular dynamics code designed for high-performance simulation of large biomolecular systems.  NAMD scales to hundreds of cores for typical simulations, however NAMD calculations are restricted to at most a single node on the JADE service.


Job scripts
-----------

Below is an example of a NAMD job script

::

    #!/bin/bash

    #SBATCH --nodes=1
    #SBATCH --ntassk-per-node=20
    #SBATCH --job-name=testNAMD
    #SBATCH --time=01:00:00
    #SBATCH --gres=gpu:4

    module load NAMD/2.12

    $NAMDROOT/namd2 +p$SLURM_NTASKS_PER_NODE +setcpuaffinity +devices $CUDA_VISIBLE_DEVICES ./input.conf &> run.log

The above example utilises half the resources on a JADE node, with a requests for a single node with 20 tasks and 4 GPUs.

Because the job is run on a single node, NAMD can be started directly, thus avoiding the use of the launcher `charmrun`.  The application is set to run on the resources allocated uwing the `+p` and `+devices` command line options.  Additionally, affinity is requested using the option `+setcpuaffinity`.

The general recommendation is to have no more than one process per GPU in the multi-node run, allowing the full utilisation of multiple cores via multi-threading.  For single node jobs, the use of multiple GPUs per process is permitted.

To read more about NAMD processing on GPUs, please visit https://www.nvidia.com/en-us/data-center/gpu-accelerated-applications/namd/ .


Installation notes
------------------
The latest version of the source code was used and built using OpenMPI v1.10.5a1 and GCC v4.8.4 following instructions from http://www.nvidia.com/object/gpu-accelerated-applications-namd-installation.html

Charm++ was built using:

::

    ./build charm++ verbs-linux-x86_64 gcc smp --with-production

NAMD was built using the following:

::

    ./config Linux-x86_64-g++ --charm-arch verbs-linux-x86_64-smp-gcc --with-cuda --cuda-prefix /usr/local/cuda-8.0


For the decision on the number of threads to use per node, take a look at http://www.nvidia.com/object/gpu-accelerated-applications-namd-running-jobs.html
