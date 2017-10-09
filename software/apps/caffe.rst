.. _caffe:

Caffe
=====

.. sidebar:: Caffe

   :URL: http://caffe.berkeleyvision.org/

`Caffe <http://caffe.berkeleyvision.org/>`_ is a Deep Learning framework made with expression, speed, and modularity in mind. It is developed by the Berkeley Vision and Learning Center (`BVLC <http://bvlc.eecs.berkeley.edu/>`_) and by community contributors.

The Caffe Docker Container
--------------------------

Caffe is available on JADE through the use of a `Docker container <https://docker.com>`_. For more information on JADE's use of containers, see :ref:`containers`.


Using Caffe Interactively
-------------------------

All the contained applications are launched interactively in the same way within 1 compute node at a time. The number of GPUs to be used per node is requested using the “gres”  option. To request an interactive session on a compute node the following command is issued from the login node: ::

  # Requesting 2 GPUs for Caffe version 17.04
  srun --gres=gpu:2 --pty  /jmain01/apps/docker/caffe 17.04

This command will show the following, which is now running on a compute node: ::

  ==================
  == NVIDIA Caffe ==
  ==================

  NVIDIA Release 17.04 (build 26740)

  Container image Copyright (c) 2017, NVIDIA CORPORATION.  All rights reserved.
  Copyright (c) 2014, 2015, The Regents of the University of California (Regents)
  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  groups: cannot find name for group ID 1002
  I have no name!@124cf0e3582e:/home_directory$

You are now inside the container where the `Caffe` software is installed. Let's check the version ::

  caffe --version

You can now begin training your network: ::

  caffe train -solver=my_solver.prototxt

Using Caffe in Batch Mode
-------------------------

There are wrappers for launching the containers within batch mode.

Firstly navigate to the folder you wish your script to lauch from, for example we'll use the home directory: ::

  cd ~

It is recommended that you create a script file e.g. `script.sh`: ::

  #!/bin/bash

  # Prints out Caffe's version number
  caffe --version

And don't forget to make your `script.sh` executable: ::

  chmod +x script.sh

Then create a Slurm batch script that is used to launch the code, e.g. `batch.sh`: ::

  #!/bin/bash
  #SBATCH --nodes=1
  #SBATCH -p all
  #SBATCH -J Caffe-job
  #SBATCH --gres=gpu:8
  #SBATCH --time=01:00:00

  #Launching the commands within script.sh
  /jmain01/apps/docker/caffe-batch –c ./script.sh

You can then submit the job using `sbatch`: ::

  sbatch batch.sh

The output will appear in the slurm standard output file.









.. _GPUComputing@Sheffield: http://gpucomputing.shef.ac.uk
