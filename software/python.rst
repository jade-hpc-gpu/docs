.. _python-conda:

Python
======

.. sidebar:: Python

   :URL: https://python.org
   :URL: https://www.anaconda.com


This page documents the python and Anaconda installation on JADE. This is the
recommended way of using Python, and the best way to be able to configure custom
sets of packages for your use.

"conda" a Python package manager, allows you to create "environments" which are
sets of packages that you can modify. It does this by installing them in your
home area. This page will guide you through loading conda and then creating and
modifying environments so you can install and use whatever Python packages you
need.

Using standard Python
---------------------

Standard Python 2 and 3 are available to be loaded as a module: ::

  python2/2.7.14
  python3/3.6.3

Use the ``module load`` command to load a particular version of python e.g. for Python 2.7.14: ::

  module load python2/2.7.14

Using conda Python
------------------

Conda version ``4.3.30`` is available for both Python 2 and 3 and can be loaded through provided module files: ::

  python2/anaconda
  python3/anaconda

Use the ``module load`` command to load a particular Anaconda Python version e.g. Anaconda for Python 3: ::

  module load python/anaconda3


Using conda Environments
########################

There are a small number of environments provided for everyone to use, these are
the default ``root`` and ``python2`` environments as well as various versions
of Anaconda for Python 3 and Python 2.

Once the conda module is loaded you have to load or create the desired
conda environments. For the documentation on conda environments see
`the conda documentation <http://conda.pydata.org/docs/using/envs.html>`_.

You can load a conda environment with::

    source activate python2

where ``python2`` is the name of the environment, and unload one with::

    source deactivate

which will return you to the ``root`` environment.

It is possible to list all the available environments with::

    conda env list

Provided system-wide are a set of anaconda environments, these will be
installed with the anaconda version number in the environment name, and never
modified. They will therefore provide a static base for derivative environments
or for using directly.


Creating an Environment
#######################

Every user can create their own environments, and packages shared with the
system-wide environments will not be reinstalled or copied to your file store,
they will be ``symlinked``, this reduces the space you need in your ``/home``
directory to install many different Python environments.

To create a clean environment with just Python 2 and numpy you can run::

    conda create -n mynumpy python=2.7 numpy

This will download the latest release of Python 2.7 and numpy, and create an
environment named ``mynumpy``.

Any version of Python or list of packages can be provided::

    conda create -n myscience python=3.5 numpy=1.8.1 scipy

If you wish to modify an existing environment, such as one of the anaconda
installations, you can ``clone`` that environment::

    conda create --clone anaconda3-4.2.0 -n myexperiment

This will create an environment called ``myexperiment`` which has all the
anaconda 4.2.0 packages installed with Python 3.


Installing Packages Inside an Environment
#########################################

Once you have created your own environment you can install additional packages
or different versions of packages into it. There are two methods for doing
this, ``conda`` and ``pip``, if a package is available through conda it is
strongly recommended that you use conda to install packages. You can search for
packages using conda::

    conda search pandas

then install the package using::

    conda install pandas

if you are not in your environment you will get a permission denied error
when trying to install packages, if this happens, create or activate an
environment you own.

If a package is not available through conda you can search for and install it
using pip, *i.e.*::

    pip search colormath

    pip install colormath
