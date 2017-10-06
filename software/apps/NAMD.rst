.. _namd:

NAMD
====

The latest version of the source code was used and built using OpenMPI v1.10.5a1 and GCC v4.8.4 following instructions from: http://www.nvidia.com/object/gpu-accelerated-applications-namd-installation.html

Charm++ was built using:

::

    ./build charm++ verbs-linux-x86_64 gcc smp --with-production

NAMD was built using the following:

::

    ./config Linux-x86_64-g++ --charm-arch verbs-linux-x86_64-smp-gcc --with-cuda --cuda-prefix /usr/local/cuda-8.0


For the decision on the number of threads to use per node, take a look at: http://www.nvidia.com/object/gpu-accelerated-applications-namd-running-jobs.html

::

    #!/bin/bash
    #SBATCH --nodes=2
    #SBATCH -p all
    #SBATCH -J NAMD
    #SBATCH --time=01:00:00
    #SBATCH --gres=gpu:8

    module load NAMD/2.12

    #set up the nodelist file
    srun hostname > hf-1
    sed 's/^/host /' hf-1 > hf && rm hf-1
    echo 'group main ++shell ssh' | cat - hf  > hf-1 && mv hf-1 hf

    $NAMDROOT/charmrun ++p 64 ++ppn 4 $NAMDROOT/namd2
    ++nodelist hf +setcpuaffinity +pemap 0-19,20-39 +commap 0,20
    +devices 3,1 src/alanin

    rm hf

As can be seen from the script setting above the code will run across multiple nodes, in this case 2 nodes.
