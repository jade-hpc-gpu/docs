.. _tensorflow:

Tensorflow
==========

.. sidebar:: Tensorflow

   :URL: https://www.tensorflow.org/

TensorFlow is an open source software library for numerical computation using data flow graphs. Nodes in the graph represent mathematical operations, while the graph edges represent the multidimensional data arrays (tensors) communicated between them. The flexible architecture allows you to deploy computation to one or more CPUs or GPUs in a desktop, server, or mobile device with a single API. TensorFlow was originally developed by researchers and engineers working on the Google Brain Team within Google's Machine Intelligence research organization for the purposes of conducting machine learning and deep neural networks research, but the system is general enough to be applicable in a wide variety of other domains as well.

About Tensorflow on ShARC
-------------------------

**A GPU-enabled worker node must be requested in order to use the GPU version of this software. See** :ref:`GPUComputing_sharc` **for more information.**

Tensorlfow is available on ShARC as both Singularity images and by local installation.

As Tensorflow and all its dependencies are written in Python, it can be installed locally in your home directory. The use of Anaconda (:ref:`sharc-python-conda`) is recommended as it is able to create a virtual environment in your home directory, allowing the installation of new Python packages without admin permission.

This software and documentation is maintained by the `RSES group <http://rse.shef.ac.uk/>`_ and `GPUComputing@Sheffield <http://gpucomputing.shef.ac.uk/>`_. For feature requests or if you encounter any problems, please raise an issue on the `GPU Computing repository <https://github.com/RSE-Sheffield/GPUComputing/issues>`_.

Tensorflow Singularity Images
-----------------------------

Singularity images are self-contained virtual machines similar to Docker. For more information on Singularity and how to use the images, see :ref:`singularity_sharc`.

A symlinked file is provided that always point to the latest image: ::

  #CPU Tensorflow
  /usr/local/packages/singularity/images/tensorflow/cpu.img

  #GPU Tensorflow
  /usr/local/packages/singularity/images/tensorflow/gpu.img

To get a bash terminal in to an image for example, use the command: ::

  singularity exec /usr/local/packages/singularity/images/tensorflow/gpu.img /bin/bash

The ``exec`` command can also be used to call any command/script inside the image e.g. ::

  singularity exec /usr/local/packages/singularity/images/tensorflow/gpu.img python your_tensorflow_script.py

You may get a warning similar to ``groups: cannot find name for group ID ...``, this can be ignored and will not have an affect on running the image.

The paths ``/fastdata``, ``/data``, ``/home``, ``/scratch``, ``/shared`` are automatically mounted to your ShARC filestore directories. For GPU-enabled images the ``/nvlib`` and ``/nvbin`` is mounted to the correct Nvidia driver version for the node that you're using.

Tensorflow is installed as part of Anaconda and can be found inside the image at: ::

  /usr/local/anaconda3-4.2.0/lib/python3.5/site-packages/tensorflow


**To submit jobs that uses a Singularity image, see** :ref:`use_image_batch_singularity_sharc` **for more detail.**

Image Index
^^^^^^^^^^^

Paths to the actual images and definition files are provided below for downloading and building of custom images.

* Shortcut to Latest Image
    * CPU
        * ``/usr/local/packages/singularity/images/tensorflow/cpu.img``
    * GPU
        * ``usr/local/packages/singularity/images/tensorflow/gpu.img``
* CPU Images
    * Latest: 1.0.1-CPU-Ubuntu16.04-Anaconda3.4.2.0.img (GCC 5.4.0, Python 3.5)
        * Path: ``/usr/local/packages/singularity/images/tensorflow/1.0.1-CPU-Ubuntu16.04-Anaconda3.4.2.0.img``
        * Def file: :download:`/sharc/software/apps/singularity/tensorflow_cpu.def </sharc/software/apps/singularity/tensorflow_cpu.def>`
* GPU Images
    * Latest: 1.0.1-GPU-Ubuntu16.04-Anaconda3.4.2.0-CUDA8-cudNN5.0.img (GCC 5.4.0, Python 3.5)
        * Path: ``/usr/local/packages/singularity/images/tensorflow/1.0.1-GPU-Ubuntu16.04-Anaconda3.4.2.0-CUDA8-cudNN5.0.img``
        * Def file: :download:`/sharc/software/apps/singularity/tensorflow_gpu.def </sharc/software/apps/singularity/tensorflow_gpu.def>`
    * Latest: 1.0.1-GPU-Ubuntu16.04-Anaconda3.4.2.0-CUDA8-cudNN5.0.img (GCC 5.4.0, Python 3.5)
        * Path: ``/usr/local/packages/singularity/images/tensorflow/1.0.1-GPU-Ubuntu16.04-Anaconda3.4.2.0-CUDA8-cudNN5.0.img``
        * Def file: :download:`/sharc/software/apps/singularity/tensorflow_gpu.def </sharc/software/apps/singularity/tensorflow_gpu.def>`

Installation in Home Directory
------------------------------

The following is an instruction on how to setup Tensorflow on your user account.

First request an interactive session, e.g. with :ref:`qrshx`. To use GPUs see :ref:`GPUInteractive_sharc`.

Load the relevant modules (our example uses CUDA 8.0 with cuDNN 5.1 but :ref:`other versions are available <cudnn_sharc>`) ::

	module load apps/python/anaconda3-4.2.0
	module load libs/cudnn/5.1/binary-cuda-8.0.44


Create a conda environment to load relevant modules on your local user account and activate it ::

	conda create -n tensorflow python=3.5
	source activate tensorflow

Then install tensorflow with the following commands ::

	export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.11.0-cp35-cp35m-linux_x86_64.whl
	pip install $TF_BINARY_URL

You can test that Tensorflow is running on the GPU with the following python code ::

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

Which gives the following results ::

	[[ 22.  28.]
	 [ 49.  64.]]

Every Session Afterwards and in Your Job Scripts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The previous instuctions installs Tensorflow and its dependencies inside your home directory but every time you use a new session or within your job scripts, the modules must be loaded and conda must be activated again. Use the following command to activate the Conda environment with Tensorflow installed: ::

	module load apps/python/anaconda3-4.2.0
	module load libs/cudnn/5.1/binary-cuda-8.0.44
	source activate tensorflow

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

You will see the following output. ::

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
