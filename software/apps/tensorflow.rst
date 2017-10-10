.. _tensorflow:

Tensorflow
==========

.. sidebar:: Tensorflow

   :URL: https://www.tensorflow.org/

TensorFlow is an open source software library for numerical computation using data flow graphs. Nodes in the graph represent mathematical operations, while the graph edges represent the multidimensional data arrays (tensors) communicated between them. The flexible architecture allows you to deploy computation to one or more CPUs or GPUs in a desktop, server, or mobile device with a single API. TensorFlow was originally developed by researchers and engineers working on the Google Brain Team within Google's Machine Intelligence research organization for the purposes of conducting machine learning and deep neural networks research, but the system is general enough to be applicable in a wide variety of other domains as well.

Tensorflow Docker Container
-----------------------

Tensorflow is available on JADE through the use of a `Docker container <https://docker.com>`_. For more information on JADE's use of containers, see :ref:`containers`.

Using Tensorflow Interactively
------------------------------

All the contained applications are launched interactively in the same way within 1 compute node at a time. The number of GPUs to be used per node is requested using the “gres”  option. To request an interactive session on a compute node the following command is issued from the login node: ::

  # Requesting 2 GPUs for Tensorflow image version 17.07
  srun --gres=gpu:2 --pty  /jmain01/apps/docker/tensorflow 17.07

This command will show the following, which is now running on a compute node: ::

  ================
  == TensorFlow ==
  ================

  NVIDIA Release 17.07 (build 84991)

  Container image Copyright (c) 2017, NVIDIA CORPORATION.  All rights reserved.
  Copyright 2017 The TensorFlow Authors.  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  groups: cannot find name for group ID 30773
  I have no name!@d129dbb678f2:/home_directory$

.. note::

  The group ID warning and no name warning can safely be ignored.

.. note::

  Inside the container, your home directory on the outside e.g. `/jmain01/home/JAD00X/test/test1-test` is mapped to the `/home_directory` folder inside the container.

  You can test this by using the command:
    ls /home_directory

You can test that `Tensorflow` is running on the GPU with the following python code `tftest.py` ::

  import tensorflow as tf
  # Creates a graph.
  with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')
    c = tf.matmul(a, b)
  # Creates a session with log_device_placement set to True.
  sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
  # Runs the op.
  print(sess.run(c))

Run the `tftest.py` script with the following command: ::

  python tftest.py

Which gives the following results: ::

	[[ 22.  28.]
	 [ 49.  64.]]

Using Tensorflow in Batch Mode
------------------------------

There are wrappers for launching the containers within batch mode.

Firstly navigate to the folder you wish your script to lauch from, for example we'll use the home directory: ::

  cd ~

It is recommended that you create a **script file** e.g. `script.sh`: ::

  #!/bin/bash

  # Run the tftest.py script, see previous section for contents
  python tftest.py

And don't forget to make your `script.sh` executable: ::

  chmod +x script.sh

Then create a **Slurm batch script** that is used to launch the code, e.g. `batch.sh`: ::

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
  /jmain01/apps/docker/tensorflow-batch -c ./script.sh

You can then submit the job using `sbatch`: ::

  sbatch batch.sh

On successful submission, a job ID is given: ::

  Submitted batch job 7800

The output will appear in the slurm standard output file with the corresponding job ID (in this case `slurm-7800.out`). The content of the output is as follows: ::

  ================
  == TensorFlow ==
  ================

  NVIDIA Release 17.07 (build 84991)

  Container image Copyright (c) 2017, NVIDIA CORPORATION.  All rights reserved.
  Copyright 2017 The TensorFlow Authors.  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  [[ 22.  28.]
	 [ 49.  64.]]


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
	# Creates a session with log_device_placement set to True.
	sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
	# Runs the op.
	print sess.run(sum)

You will see something similar to the following output. ::

	Device mapping:
	/job:localhost/replica:0/task:0/gpu:0 -> device: 0, name: Tesla K20m, pci bus
	id: 0000:02:00.0
	/job:localhost/replica:0/task:0/gpu:1 -> device: 1, name: Tesla K20m, pci bus
	id: 0000:03:00.0
	/job:localhost/replica:0/task:0/gpu:2 -> device: 2, name: Tesla K20m, pci bus
	id: 0000:83:00.0
	/job:localhost/replica:0/task:0/gpu:3 -> device: 3, name: Tesla K20m, pci bus
	id: 0000:84:00.0
	Const_3: /job:localhost/replica:0/task:0/gpu:3
	Const_2: /job:localhost/replica:0/task:0/gpu:3
	MatMul_1: /job:localhost/replica:0/task:0/gpu:3
	Const_1: /job:localhost/replica:0/task:0/gpu:2
	Const: /job:localhost/replica:0/task:0/gpu:2
	MatMul: /job:localhost/replica:0/task:0/gpu:2
	AddN: /job:localhost/replica:0/task:0/cpu:0
	[[  44.   56.]
	 [  98.  128.]]
