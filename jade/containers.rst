.. _containers:

Using Containerised Applications
================================

On entering the container the present working directory will be the user's home directory: ``/home_directory``

Any files you copy into ``/home_directory`` will have the same userid as normal and will be available once exiting the container. The local disk space on the node is available at: ``/local_scratch/$USERID``

This is 6.6TB in size but any data will be lost once the interactive session is ended. There are two ways of interacting with the containerised applications.

1.Interactive Mode
------------------

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

2.Batch Mode
------------

There are wrappers for launching the containers in batch mode. For example, to launch the Torch application change directory to where the launching script is, in this case called ``submit-char.sh``: 

::

    cd /jmain01/home/atostest/char-rnn-master

A Slurm batch script is used to launch the code, such as:

::

    #!/bin/bash
    #SBATCH --nodes=1
    #SBATCH -p all
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
