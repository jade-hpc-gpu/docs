.. _containers:

Using Containerised Applications
================================

On entering the container the present working directory will be the user's home directory: ``/home_directory``

Any files you copy into ``/home_directory`` will have the same userid as normal and will be available once exiting the container. The local disk space on the node is available at: ``/local_scratch/``

This is 6.6TB in size but any data will be lost once the interactive session is ended. There are two ways of interacting with the containerised applications.

Docker Containers
-----------------


Listing available containers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To list the containers and version available on the system do:

::

    -bash-4.2$ containers
    REPOSITORY                   TAG                     IMAGE ID       CREATED         SIZE
    nvcr.io/nvidia/tensorflow    20.11-tf2-py3           98a7952f7f9c   5 weeks ago     11.6GB
    nvcr.io/nvidia/pytorch       20.11-py3               ae35b2b3cad1   5 weeks ago     13.2GB
    nvcr.io/nvidia/l4t-pytorch   r32.4.4-pth1.6-py3      78f267ed1c22   8 weeks ago     2.2GB
    nvcr.io/hpc/namd             3.0-alpha3-singlenode   205dd048d054   5 months ago    1.24GB
    nvcr.io/hpc/gromacs          2020.2                  c8a188675719   5 months ago    570MB
    nvcr.io/nvidia/caffe         20.03-py3               aa6834df762b   9 months ago    4.82GB
    nvcr.io/nvidia/caffe2        18.08-py3               e82334d03b18   2 years ago     3.02GB
    nvcr.io/nvidia/theano        18.08                   1462ba2d70fe   2 years ago     3.7GB
    nvcr.io/nvidia/torch         18.08-py2               889c9b4d3b71   2 years ago     3.06GB



Interactive Mode
~~~~~~~~~~~~~~~~

All the applications in containers can be launched interactively in the same way using 1 compute node at a time. The number of GPUs to be used per node is requested using the ``gres`` option. To request an interactive session on a compute node the following command is issued from the login node:

::

    srun --gres=gpu:2 --pty /jmain02/apps/docker/pytorch 20.11-py3

This command will show the following, which is now running on a compute node:

::

    =============
    == PyTorch ==
    =============

    NVIDIA Release 20.11 (build 17345815)
    PyTorch Version 1.8.0a0+17f8c32

    Container image Copyright (c) 2020, NVIDIA CORPORATION.  All rights reserved.

    Copyright (c) 2014-2020 Facebook Inc.
    Copyright (c) 2011-2014 Idiap Research Institute (Ronan Collobert)
    Copyright (c) 2012-2014 Deepmind Technologies    (Koray Kavukcuoglu)
    Copyright (c) 2011-2012 NEC Laboratories America (Koray Kavukcuoglu)
    Copyright (c) 2011-2013 NYU                      (Clement Farabet)
    Copyright (c) 2006-2010 NEC Laboratories America (Ronan Collobert, Leon Bottou, Iain Melvin, Jason Weston)
    Copyright (c) 2006      Idiap Research Institute (Samy Bengio)
    Copyright (c) 2001-2004 Idiap Research Institute (Ronan Collobert, Samy Bengio, Johnny Mariethoz)
    Copyright (c) 2015      Google Inc.
    Copyright (c) 2015      Yangqing Jia
    Copyright (c) 2013-2016 The Caffe contributors
    All rights reserved.

    NVIDIA Deep Learning Profiler (dlprof) Copyright (c) 2020, NVIDIA CORPORATION.  All rights reserved.

    Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
    NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

    NOTE: Legacy NVIDIA Driver detected.  Compatibility mode ENABLED.

    groups: cannot find name for group ID 31196
    I have no name!@085a165a38ee:/home_directory$


Note. The warnings in the last two lines can be ignored. To exit the container, issue the "exit" command. To launch the other containers the commands are:

::

    srun --gres=gpu:8 --pty /jmain02/apps/docker/theano 18.08
    srun --gres=gpu:4 --pty /jmain02/apps/docker/torch 18.08-py2

Batch Mode
~~~~~~~~~~

There are wrappers for launching the containers in batch mode. For example, to launch the Torch application:

A Slurm batch script is used to launch the code, such as:
::

    #!/bin/bash
    #SBATCH --nodes=1
    #SBATCH -J Torch
    #SBATCH --gres=gpu:8
    #SBATCH --time=01:00:00

    /jmain02/apps/docker/torch-batch -c ./submit-char.sh

The output will appear in the slurm standard output file.

Each of the containerised applications has its own batch launching script:

::

    /jmain02/apps/docker/torch-batch
    /jmain02/apps/docker/caffe-batch
    /jmain02/apps/docker/theano-batch


Singularity Containers
----------------------

Singularity 3.7 is installed on compute nodes, it is not available on login nodes. When you build your container, within your own environment, your container you must have the following directories:

::

    /tmp
    /local_scratch


These will be mounted by the local node when your container executes. The ``/tmp`` & ``/local_scratch`` directory are the local RAID disks on the DGX node and should be used for building code or temporary files. 

Unlike Docker containers, the home directory the same as when you're outside the container (e.g. ``/jmain02/home/your_project/your_group/your_username``). You can use ``cd ~`` to get to your home directory and ``echo $HOME`` to print out your home location.

There are 2 scripts in the ``/jmain02/apps/singularity`` directory that you can use to launch your container using Slurm:

::

    singbatch
    singinteractive

You call them with either

::

    singinteractive CONTAINER_FILE
    # OR
    singbatch CONTAINER_FILE SCRIPT_TO_EXECUTE


You should use these scripts with Slurm. So for example with an INTERACTIVE session:

::

    srun -I --pty -t 0-10:00 --gres gpu:1 -p small singularity /jmain02/apps/singularity/singularity-images/caffe-gpu.img

If you want to run in batch mode, you should call ``singbatch`` (using sbatch) and provide a script to execute within the container.

You MUST respect the ``CUDA_VISIBLE_DEVICES`` variable within the container, as you can see ALL the GPUs in the container. Some of these GPUs may be in use by other users and Slurm has allocated you a specific ones/group & will set this variable for you. If you are familiar with Docker, it only shows you the GPUs have been allocated.

Slurm will clear out ``/tmp`` and ``/local_scratch`` once you exit the container, so make sure you copy anything back to your home directory if you need it! There is an example “caffe” image provided in ``/jmain01/apps/singularity/singularity-images`` if you wish to contribute an image for others to use, please submit an issue to the `Github Issue tracker <https://github.com/jade-hpc-gpu/jade-hpc-gpu.github.io/issues>`_



