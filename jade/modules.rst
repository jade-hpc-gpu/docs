.. _modules:

The ``module`` tool
===================

Introduction
------------

The GNU/Linux operating system makes extensive use of the *working environment*, which is a collection of individual environment variables.  An environment variable is a named object in a shell that contains information used by one or more applications; two of the most used such variables are ``$HOME``, which defines a user's home directory name, and ``$PATH``, which represents a list paths to different executables.  A large number of environment variables are already defined when a shell is open but the environment can be customised, either by defining new environment variables relevant to certain applications or by modifying existing ones (e.g. adding a new path to ``$PATH``).

``module`` is a Software Environment Management tool, which is used to manage the working environment in preparation for running the applications installed on JADE.  By loading the module for a certain installed application, the environment variables that are relevant for that application are automatically defined or modified.

Useful commands
---------------

The module utility is invoked by the command ``module``.  This command must be followed by an instruction of what action is required and by the details of a pre-defined module.

The utility displays a help menu by doing::

  module help

The utility displays the available modules by issuing the command::

  module avail

or displays only the information related to a certain software package, *e.g.*::

  module avail gromacs

The avail instruction displays all the versions available for the installed applications, and shows which version is pre-defined as being the default. A software package is loaded with the load or the add instructions, *e.g.*::

  module load gromacs

If no version is specified, the default version of the software is loaded. Specific versions, other than the default can be loaded by specifying the version, *e.g.*::

  module load gromacs/2020.3

The modules that are already loaded by users in a session are displayed with the command::

  module list

A module can be "unloaded" with the unload or rm instructions, *e.g.*::

  module unload gromacs
  module load gromacs/2020.3

Lastly, all modules loaded in a session can be "unloaded" with a single command:::

  module purge


Best practices
--------------

``module`` can be used to modify the environment after login, *e.g.* to load the Portland compilers in order to build an application.  However, most frequent usage will be to load an already built application, and the best way to do this is from within the submission script.  For example, assuming a job uses the NAMD molecular dynamics package, the submission script contains::

  module purge
  module load namd
