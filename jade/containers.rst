.. _containers:

Using Containerised Applications
================================

On entering the container the present working directory will be the user's home directory: ``/home_directory``

Any files you copy into ``/home_directory`` will have the same userid as normal and will be available once exiting the container. The local disk space on the node is available at: ``/local_scratch/$USERID``

This is 6.6TB in size but any data will be lost once the interactive session is ended. There are two ways of interacting with the containerised applications.

Docker Containers
----------------------------


Listing available containers
~~~~~~~~~~~~~~~~~~~~~~

To list the containers and version available on the system do:

::

    root@dgj223:~# containers
    REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
    nvcr.io/nvidia/caffe        17.11               74f90888fb24        4 weeks ago         3.247 GB
    nvcr.io/nvidia/theano       17.11               39aed30f94ed        6 weeks ago         3.367 GB
    nvcr.io/nvidia/torch        17.11               1fb9e886f48c        6 weeks ago         3.267 GB
    nvcr.io/nvidia/caffe2       17.10               2ff2ccd2c8c1        10 weeks ago        2.731 GB
    nvcr.io/nvidia/tensorflow   17.07               94b1afe1821c        5 months ago        4.404 GB
    nvidia/cuda                 latest              15e5dedd88c5        7 months ago        1.67 GB
    nvcr.io/nvidia/caffe        17.04               87c288427f2d        7 months ago        2.794 GB
    nvcr.io/nvidia/theano       17.04               24943feafc9b        8 months ago        2.386 GB
    nvcr.io/nvidia/torch        17.04               a337ffb42c8e        8 months ago        2.9 GB
    Last updated:Tue Dec 19 01:00:01 GMT 2017


Interactive Mode
~~~~~~~~~~~~~~~~~~~~~~

All the applications in containers can be launched interactively in the same way using 1 compute node at a time. The number of GPUs to be used per node is requested using the ``gres`` option. To request an interactive session on a compute node the following command is issued from the login node:

::

    srun --gres=gpu:2 --pty /jmain01/apps/docker/caffe 17.04

This command will show the following, which is now running on a compute node:

::

    ================
    ==NVIDIA Caffe==
    ================

    NVIDIA Release 17.04 (build 26740)

    Container image Copyright (c) 2017, NVIDIA CORPORATION.  All rights reserved.
    Copyright (c) 2014, 2015, The Regents of the University of California (Regents)
    All rights reserved.

    Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
    NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

    groups: cannot find name for group ID 1002
    I have no name!@124cf0e3582e:/home_directory$

Note. The warnings in the last two lines can be ignored. To exit the container, issue the "exit" command. To launch the other containers the commands are:

::

    srun --gres=gpu:8 --pty /jmain01/apps/docker/theano 17.04
    srun --gres=gpu:4 --pty /jmain01/apps/docker/torch 17.04

Batch Mode
~~~~~~~~~~~~~~~~~~~~~~

There are wrappers for launching the containers in batch mode. For example, to launch the Torch application change directory to where the launching script is, in this case called ``submit-char.sh``:

::

    cd /jmain01/home/atostest/char-rnn-master

A Slurm batch script is used to launch the code, such as:

::

    #!/bin/bash
    #SBATCH --nodes=1
    #SBATCH -J Torch
    #SBATCH --gres=gpu:8
    #SBATCH --time=01:00:00

    /jmain01/apps/docker/torch-batch -c ./submit-char.sh

The output will appear in the slurm standard output file.

Each of the containerised applications has its own batch launching script:

::

    /jmain01/apps/docker/torch-batch
    /jmain01/apps/docker/caffe-batch
    /jmain01/apps/docker/theano-batch


Singularity Containers
----------

Singularity 2.4 is installed in ``/jmain01/apps/singularity/2.4``. When you build your container, within your own environment, your container you must have the following directories:

::

    /usr/local/cuda
    /tmp
    /local_scratch
    /home_directory


These will be mounted by the local node when your container executes. The ``/tmp`` & ``/local_scratch`` directory are the local RAID disks on the DGX node and should be used for building code or temporary files. Your home directory will also be mounted into the container as ``/home_directory``.

There are 2 scripts in the ``/jmain01/apps/singularity/2.4/bin`` directory that you can use to launch your container using Slurm:

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

    module load singularity
    srun -I --pty -t 0-10:00 --gres gpu:1 -p small singinteractive /jmain01/apps/singularity/singularity-images/caffe-gpu.img

If you want to run in batch mode, you should call ``singbatch`` (using sbatch) and provide a script to execute within the container.

You MUST respect the ``CUDA_VISIBLE_DEVICES`` variable within the container, as you can see ALL the GPUs in the container. Some of these GPUs may be in use by other users and Slurm has allocated you a specific ones/group & will set this variable for you. If you are familiar with Docker, it only shows you the GPUs have been allocated.

Slurm will clear out ``/tmp`` and ``/local_scratch`` once you exit the container, so make sure you copy anything back to your home directory if you need it! There is an example “caffe” image provided in ``/jmain01/apps/singularity/singularity-images`` if you wish to contribute an image for others to use, please submit an issue to the Github Issue t

