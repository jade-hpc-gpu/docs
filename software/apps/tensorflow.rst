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

.. TODO add info about using interactively

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

Using Tensorflow in Batch Mode
------------------------------







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
