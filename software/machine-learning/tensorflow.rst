.. _tensorflow:

Tensorflow
==========

.. sidebar:: Tensorflow

   :URL: https://www.tensorflow.org/ 

TensorFlow is an open source software library for numerical computation using data flow graphs. Nodes in the graph represent mathematical operations, while the graph edges represent the multidimensional data arrays (tensors) communicated between them. The flexible architecture allows you to deploy computation to one or more CPUs or GPUs in a desktop, server, or mobile device with a single API. TensorFlow was originally developed by researchers and engineers working on the Google Brain Team within Google's Machine Intelligence research organization for the purposes of conducting machine learning and deep neural networks research, but the system is general enough to be applicable in a wide variety of other domains as well.

Tensorflow Docker Container
---------------------------

Tensorflow is available on JADE through the use of a `Docker container <https://docker.com>`_. For more information on JADE's use of containers, see :ref:`containers`.

Using Tensorflow Interactively
------------------------------

All the contained applications are launched interactively in the same way within 1 compute node at a time. The number of GPUs to be used per node is requested using the “gres”  option. To request an interactive session on a compute node the following command is issued from the login node: ::

  # Requesting 2 GPUs for Tensorflow image version 20.11-tf2-py3 
  srun --gres=gpu:2 --pty  /jmain02/apps/docker/tensorflow 20.11-tf2-py3 

This command will show the following, which is now running on a compute node: ::

  ================
  == TensorFlow ==
  ================

  NVIDIA Release 20.11-tf2 (build 17379986)
  TensorFlow Version 2.3.1

  Container image Copyright (c) 2020, NVIDIA CORPORATION.  All rights reserved.
  Copyright 2017-2020 The TensorFlow Authors.  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  NOTE: Legacy NVIDIA Driver detected.  Compatibility mode ENABLED.

  groups: cannot find name for group ID 30773
  I have no name!@d129dbb678f2:/home_directory$

.. note::

  The group ID warning and no name warning can safely be ignored.

.. note::

  Inside the container, your home directory on the outside e.g. ``/jmain02/home/JAD00X/test/test1-test`` is mapped to the ``/home_directory`` folder inside the container.

  You can test this by using the command:
    ls /home_directory

You can test that ``Tensorflow`` is running on the GPU with the following python code ``tftest.py`` ::

  import tensorflow as tf
  # Creates a graph.
  with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
    c = tf.matmul(a, b)
  # Runs the op.
  print(c)

Run the ``tftest.py`` script with the following command: ::

  python tftest.py

Which gives the following results: ::

  tf.Tensor(
  [[22. 28.] [49. 64.]], shape=(2, 2), dtype=float32)


Using Tensorflow in Batch Mode
------------------------------

There are wrappers for launching the containers within batch mode.

Firstly navigate to the folder you wish your script to lauch from, for example we'll use the home directory: ::

  cd ~

It is recommended that you create a **script file** e.g. ``script.sh``: ::

  #!/bin/bash

  # Run the tftest.py script, see previous section for contents
  python tftest.py

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
  /jmain02/apps/docker/tensorflow-batch -c ./script.sh

You can then submit the job using ``sbatch``: ::

  sbatch batch.sh

On successful submission, a job ID is given: ::

  Submitted batch job 7800

The output will appear in the slurm standard output file with the corresponding job ID (in this case ``slurm-7800.out``). The content of the output is as follows: ::

  ================
  == TensorFlow ==
  ================

  NVIDIA Release 20.11-tf2 (build 17379986)
  TensorFlow Version 2.3.1

  Container image Copyright (c) 2020, NVIDIA CORPORATION.  All rights reserved.
  Copyright 2017-2020 The TensorFlow Authors.  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  NOTE: Legacy NVIDIA Driver detected.  Compatibility mode ENABLED.

  [[ 22.  28.][ 49.  64.]]


Using multiple GPUs
-------------------

Example taken from `tensorflow documentation <https://www.tensorflow.org/versions/r0.11/how_tos/using_gpu/index.html>`_.

If you would like to run TensorFlow on multiple GPUs, you can construct your model in a multi-tower fashion where each tower is assigned to a different GPU. For example: ::

	import tensorflow as tf
	# Creates a graph.
	c = []
	for d in ['/gpu:2', '/gpu:3']:
	  with tf.device(d):
	    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3])
	    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2])
	    c.append(tf.matmul(a, b))
	with tf.device('/cpu:0'):
	  sum = tf.add_n(c)
	# Runs the op.
	print (sum)

You will see something similar to the following output. ::

	I tensorflow/core/common_runtime/gpu/gpu_device.cc:1428] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 4322 MB memory) -> physical GPU (device: 0, name: Tesla V100-SXM2-32GB-LS, pci bus id: 0000:06:00.0, compute capability: 7.0)
	I tensorflow/core/common_runtime/gpu/gpu_device.cc:1428] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:1 with 31031 MB memory) -> physical GPU (device: 1, name: Tesla V100-SXM2-32GB-LS, pci bus id: 0000:07:00.0, compute capability: 7.0)
	I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublas.so.11

	tf.Tensor(
  	[[ 44.  56.][ 98. 128.]], shape=(2, 2), dtype=float32)

