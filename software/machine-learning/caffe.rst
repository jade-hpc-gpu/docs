.. _caffe:

Caffe
=====

.. sidebar:: Caffe

   :URL: http://caffe.berkeleyvision.org/

`Caffe <http://caffe.berkeleyvision.org/>`__ is a Deep Learning framework made with expression, speed, and modularity in mind. It is developed by the Berkeley Vision and Learning Center (`BVLC <http://bvlc.eecs.berkeley.edu/>`_) and by community contributors.

The Caffe Docker Container
--------------------------

Caffe is available on JADE through the use of a `Docker container <https://docker.com>`__. For more information on JADE's use of containers, see :ref:`containers`.


Using Caffe Interactively
-------------------------

All the contained applications are launched interactively in the same way within 1 compute node at a time. The number of GPUs to be used per node is requested using the “gres”  option. To request an interactive session on a compute node the following command is issued from the login node: ::

  # Requesting 2 GPUs for Caffe image version 20.03-py3
  srun --gres=gpu:2 --pty  /jmain02/apps/docker/caffe 20.03-py3

This command will show the following, which is now running on a compute node: ::

   ==================
   == NVIDIA Caffe ==
   ==================

   NVIDIA Release 20.03 (build 11026381)
   NVIDIA Caffe Version 0.17.3

   Container image Copyright (c) 2019, NVIDIA CORPORATION.  All rights reserved.
   Copyright (c) 2014, 2015, The Regents of the University of California (Regents)
   All rights reserved.

   Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
   NVIDIA modifications are covered by the license terms that apply to the underlying project or file.


   groups: cannot find name for group ID 1002
   I have no name!@124cf0e3582e:/home_directory$

.. note::

  The group ID warning and no name warning can safely be ignored.

.. note::

  Inside the container, your home directory on the outside e.g. ``/jmain02/home/JAD00X/test/test1-test`` is mapped to the ``/home_directory`` folder inside the container.

  You can test this by using the command:
    ls /home_directory

You are now inside the container where ``Caffe`` is installed. Let's check by asking for the version: ::

  caffe --version

Where you will get something like: ::

  caffe version 0.17.3



You can now begin training your network as normal: ::

  caffe train -solver=my_solver.prototxt

Using Caffe in Batch Mode
-------------------------

There are wrappers for launching the containers within batch mode.

Firstly navigate to the folder you wish your script to lauch from, for example we'll use the home directory: ::

  cd ~

It is recommended that you create a **script file** e.g. ``script.sh``: ::

  #!/bin/bash

  # Prints out Caffe's version number
  caffe --version

And don't forget to make your ``script.sh`` executable: ::

  chmod +x script.sh

Then create a **Slurm batch script** that is used to launch the code, e.g. ``batch.sh``: ::

  #!/bin/bash

  # set the number of nodes
  #SBATCH --nodes=1

  # set max wallclock time
  #SBATCH --time=01:00:00

  # set name of job
  #SBATCH -J JobName

  # set number of GPUs
  #SBATCH --gres=gpu:8

  # mail alert at start, end and abortion of execution
  #SBATCH --mail-type=ALL

  # send mail to this address
  #SBATCH --mail-user=your.mail@yourdomain.com


  #Launching the commands within script.sh
  /jmain02/apps/docker/caffe-batch -c ./script.sh

You can then submit the job using ``sbatch``: ::

  sbatch batch.sh

On successful submission, a job ID is given: ::

  Submitted batch job 7800

The output will appear in the slurm standard output file with the corresponding job ID (in this case ``slurm-7800.out``). The content of the output is as follows: ::

  ==================
  == NVIDIA Caffe ==
  ==================

  NVIDIA Release 20.03 (build 11026381)
  NVIDIA Caffe Version 0.17.3

  Container image Copyright (c) 2019, NVIDIA CORPORATION.  All rights reserved.
  Copyright (c) 2014, 2015, The Regents of the University of California (Regents)
  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.


  caffe version 0.17.3



.. _GPUComputing@Sheffield: http://gpucomputing.shef.ac.uk
