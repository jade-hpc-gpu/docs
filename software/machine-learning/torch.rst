.. _torch:

Torch
=====

.. sidebar:: Torch

   :URL: http://torch.ch/

Torch is a scientific computing framework with wide support for machine learning algorithms that puts GPUs first. It is easy to use and efficient, thanks to an easy and fast scripting language, LuaJIT, and an underlying C/CUDA implementation.

Torch Docker Container
-----------------------

Torch is available on JADE through the use of a `Docker container <https://docker.com>`_. For more information on JADE's use of containers, see :ref:`containers`.


Using Torch Interactively
------------------------------

All the contained applications are launched interactively in the same way within 1 compute node at a time. The number of GPUs to be used per node is requested using the “gres”  option. To request an interactive session on a compute node the following command is issued from the login node: ::

  # Requesting 2 GPUs for Torch image version 18.08-py2
  srun --gres=gpu:2 --pty  /jmain02/apps/docker/torch 18.08-py2

This command will show the following, which is now running on a compute node: ::

    ______             __   |  Torch7
   /_  __/__  ________/ /   |  Scientific computing for Lua.
    / / / _ \/ __/ __/ _ \  |
   /_/  \___/_/  \__/_//_/  |  https://github.com/torch
                            |  http://torch.ch

  NVIDIA Release 18.08 (build 598611)

  Container image Copyright (c) 2018, NVIDIA CORPORATION.  All rights reserved.
  Copyright (c) 2016, Soumith Chintala, Ronan Collobert, Koray Kavukcuoglu, Clement Farabet
  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.


  groups: cannot find name for group ID 30773
  I have no name!@f1915084ec5f:/home_directory$

.. note::

  The group ID warning and no name warning can safely be ignored.

.. note::

  Inside the container, your home directory on the outside e.g. ``/jmain02/home/JAD00X/test/test1-test`` is mapped to the ``/home_directory`` folder inside the container.

  You can test this by using the command:
    ls /home_directory

You are now inside the container where ``Torch`` is installed.

Torch console
^^^^^^^^^^^^^

``Torch`` can be used interactively by using the ``th`` command: ::

  th

Where you will the torch command prompt: ::

    ______             __   |  Torch7
   /_  __/__  ________/ /   |  Scientific computing for Lua.
    / / / _ \/ __/ __/ _ \  |  Type ? for help
   /_/  \___/_/  \__/_//_/  |  https://github.com/torch
                          |  http://torch.ch

  th>

When you're done, type ``exit`` and then ``y`` to exit the ``Torch`` console:  ::

  th> exit
  Do you really want to exit ([y]/n)? y
  I have no name!@f1915084ec5f:/home_directory$


Using LUA script
^^^^^^^^^^^^^^^^

It is also possible to pass a LUA script to the ``th`` command. For example, create a ``test.lua`` file in the current directory with the contents: ::

  torch.manualSeed(1234)
  -- choose a dimension
  N = 5

  -- create a random NxN matrix
  A = torch.rand(N, N)

  -- make it symmetric positive
  A = A*A:t()

  -- make it definite
  A:add(0.001, torch.eye(N))

  -- add a linear term
  b = torch.rand(N)

  -- create the quadratic form
  function J(x)
     return 0.5*x:dot(A*x)-b:dot(x)
  end

  print(J(torch.rand(N)))


Call the ``test.lua`` script by using the command: ::

  th test.lua

Which shows the following results: ::

  0.72191523289161



Using Torch in Batch Mode
-------------------------

There are wrappers for launching the containers within batch mode.

Firstly navigate to the folder you wish your script to lauch from, for example we'll use the home directory: ::

  cd ~

It is recommended that you create a **script file** e.g. ``script.sh``: ::

  #!/bin/bash

  # Runs a script called test.lua
  # see above section for contents
  th test.lua

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
  /jmain02/apps/docker/torch-batch -c ./script.sh

You can then submit the job using ``sbatch``: ::

  sbatch batch.sh

On successful submission, a job ID is given: ::

  Submitted batch job 7800

The output will appear in the slurm standard output file with the corresponding job ID (in this case ``slurm-7800.out``). The content of the output is as follows: ::

    ______             __   |  Torch7
   /_  __/__  ________/ /   |  Scientific computing for Lua.
    / / / _ \/ __/ __/ _ \  |
   /_/  \___/_/  \__/_//_/  |  https://github.com/torch
                          |  http://torch.ch

  NVIDIA Release 18.08 (build 598611)

  Container image Copyright (c) 2018, NVIDIA CORPORATION.  All rights reserved.
  Copyright (c) 2016, Soumith Chintala, Ronan Collobert, Koray Kavukcuoglu, Clement Farabet
  All rights reserved.

  Various files include modifications (c) NVIDIA CORPORATION.  All rights reserved.
  NVIDIA modifications are covered by the license terms that apply to the underlying project or file.

  0.72191523289161
