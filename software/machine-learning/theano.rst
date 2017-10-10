.. _theano:

Theano
======

.. sidebar:: Theano

   :URL: http://deeplearning.net/software/theano/index.html

Theano is a Python library that allows you to define, optimize, and evaluate mathematical expressions involving multi-dimensional arrays efficiently. Theano is most commonly used to perform Deep Learning and has excellent GPU support and integration through PyCUDA. The following steps can be used to setup and configure Theano on your own profile.

Theano Docker Container
-----------------------

Theano is available on JADE through the use of a `Docker container <https://docker.com>`_. For more information on JADE's use of containers, see :ref:`containers`.

Using Theano Interactively
------------------------------

All the contained applications are launched interactively in the same way within 1 compute node at a time. The number of GPUs to be used per node is requested using the “gres”  option. To request an interactive session on a compute node the following command is issued from the login node: ::

  # Requesting 2 GPUs for Theano image version 17.04
  srun --gres=gpu:2 --pty  /jmain01/apps/docker/theano 17.04

This command will show the following, which is now running on a compute node: ::

  ============
  == Theano ==
  ============

  NVIDIA Release 17.04 (build 21022)

  Container image Copyright (c) 2017, NVIDIA CORPORATION.  All rights reserved.
  Copyright (c) 2008--2016, Theano Development Team
  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  groups: cannot find name for group ID 30773
  I have no name!@8ded1f005768:/home_directory$


.. note::

  The group ID warning and no name warning can safely be ignored.

.. note::

  Inside the container, your home directory on the outside e.g. ``/jmain01/home/JAD00X/test/test1-test`` is mapped to the ``/home_directory`` folder inside the container.

  You can test this by using the command:
    ls /home_directory

You can test that ``Theano`` is running on the GPU with the following python code ``theanotest.py`` ::

  from theano import function, config, shared, sandbox
  import theano.tensor as T
  import numpy
  import time

  vlen = 10 * 30 * 768  # 10 x #cores x # threads per core
  iters = 1000

  rng = numpy.random.RandomState(22)
  x = shared(numpy.asarray(rng.rand(vlen), config.floatX))
  f = function([], T.exp(x))
  print(f.maker.fgraph.toposort())
  t0 = time.time()
  for i in range(iters):
      r = f()
  t1 = time.time()
  print("Looping %d times took %f seconds" % (iters, t1 - t0))
  print("Result is %s" % (r,))
  if numpy.any([isinstance(x.op, T.Elemwise) for x in f.maker.fgraph.toposort()]):
      print('Used the cpu')
  else:
      print('Used the gpu')

Run the ``theanotest.py`` script with the following command: ::

  THEANO_FLAGS="device=gpu" python theanotest.py

The ``THEANO_FLAGS`` ``device`` variable can be set to either ``cpu`` or ``gpu``.

Which gives the following results: ::

  WARNING (theano.sandbox.cuda): The cuda backend is deprecated and will be removed in the next release (v0.10).  Please switch to the gpuarray backend. You can get more information about how to switch at this URL:
  https://github.com/Theano/Theano/wiki/Converting-to-the-new-gpu-back-end%28gpuarray%29

  Using gpu device 0: Tesla P100-SXM2-16GB (CNMeM is enabled with initial size: 90.0% of memory, cuDNN 6020)
  [GpuElemwise{exp,no_inplace}(<CudaNdarrayType(float32, vector)>), HostFromGpu(GpuElemwise{exp,no_inplace}.0)]
  Looping 1000 times took 0.230759 seconds
  Result is [ 1.23178029  1.61879349  1.52278066 ...,  2.20771813  2.29967761
    1.62323296]
  Used the gpu

.. note::

  Theano's cuda backend warning can be ignored for now.


Using Theano in Batch Mode
------------------------------

There are wrappers for launching the containers within batch mode.

Firstly navigate to the folder you wish your script to lauch from, for example we'll use the home directory: ::

  cd ~

It is recommended that you create a **script file** e.g. ``script.sh``: ::

  #!/bin/bash

  # Run the theanotest.py script, see previous section for contents
  python theanotest.py

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
  /jmain01/apps/docker/theano-batch -c ./script.sh

You can then submit the job using ``sbatch``: ::

  sbatch batch.sh

On successful submission, a job ID is given: ::

  Submitted batch job 7800

The output will appear in the slurm standard output file with the corresponding job ID (in this case ``slurm-7800.out``). The content of the output is as follows: ::

  ============
  == Theano ==
  ============

  NVIDIA Release 17.04 (build 21022)

  Container image Copyright (c) 2017, NVIDIA CORPORATION.  All rights reserved.
  Copyright (c) 2008--2016, Theano Development Team
  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  WARNING (theano.sandbox.cuda): The cuda backend is deprecated and will be removed in the next release (v0.10).  Please switch to the gpuarray backend. You can get more information about how to switch at this URL:
  https://github.com/Theano/Theano/wiki/Converting-to-the-new-gpu-back-end%28gpuarray%29

  Using gpu device 0: Tesla P100-SXM2-16GB (CNMeM is enabled with initial size: 90.0% of memory, cuDNN 6020)
  [GpuElemwise{exp,no_inplace}(<CudaNdarrayType(float32, vector)>), HostFromGpu(GpuElemwise{exp,no_inplace}.0)]
  Looping 1000 times took 0.230759 seconds
  Result is [ 1.23178029  1.61879349  1.52278066 ...,  2.20771813  2.29967761
    1.62323296]
  Used the gpu
