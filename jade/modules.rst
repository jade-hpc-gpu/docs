.. _modules:

Activating software using Environment Modules
=============================================

Overview and rationale
----------------------

'Environment Modules' are the mechanism by which much software is made available to the users of the JADE cluster.
To make a particular piece of software available a user will *load* a 'module' e.g.
on JADE, you can load a particular version of the ``gromacs`` application (version 2016.3) with: ::

    module load gromacs/2016.3

This command manipulates `environment variables <https://en.wikipedia.org/wiki/Environment_variable>`_ to make this piece of software available.
If you then want to switch to using a different version of ``gromacs`` (should another be installed on the cluster you are using) then you can
unload ``gromacs/2016.3`` and load the other.

You may wonder why modules are necessary: why not just install packages provided by the vender of the operating system installed on the cluster?
In shared high-performance computing environments such as JADE:

* users typically want control over the version of applications that is used (e.g. to give greater confidence that results of numerical simulations can be reproduced);
* users may want to use applications built using compiler X rather than compiler Y as compiler X might generate faster code and/or more accurate numerical results in certain situations;
* users may want a version of an application built with support for particular parallelisation mechanisms such as MPI for distributing work between machines, OpenMP for distributing work between CPU cores or CUDA for parallelisation on GPUs);
* users may want an application built with support for a particular library.

There is therefore a need to maintain multiple versions of the same applications on JADE.
Module files allow users to select and use the versions they need for their research.

If you switch to using a cluster other than JADE then you will likely find that environment modules are used there too.


Basic guide
-----------

You can list all (loaded and unloaded) modules on JADE using: ::

    module avail

You can then load a module using e.g.: ::

    module load gromacs/2016.3

This will only work on worker nodes and will not have an effect on login nodes as software provided using module files is not installed on the login nodes.

You can then load further modules e.g.::

    module load NAMD/2.12

If you want to stop using a module (by undoing the changes that loading that module made to your environment): ::

    module unload NAMD/2.12

or to unload all loaded modules: ::

    module purge

To learn more about what software is available on the system and discover the names of module files, you can view the online documentation for

    * :ref:`software on JADE <software>`


You can search for a module using: ::

    module avail |& grep -i somename

The name of a Module should tell you:

* the type of software (application, library, development tool (e.g. compiler), parallel computing software);
* the name and version of the software;
* the name and version of compiler that the software was built using (if applicable; not all installed software was installed from source);
* the name and version of used libraries that distinguish the different installs of a given piece of software (e.g. the version of OpenMPI an application was built with).

Some other things to be aware of:

* You can load and unload modules in both interactive and batch jobs;
* Modules may themselves load other modules.  If this is the case for a given module then it is typically noted in our documentation for the corresponding software;
* The order in which you load modules may be significant (e.g. if module A sets ``SOME_ENV_VAR=apple`` and module B sets ``SOME_ENV_VAR=pear``);
.. TODO Need to confirm the following -> * Some related module files have been set up so that they are mutually exclusive e.g.  the modules ``dev/NAG/6.0`` and ``dev/NAG/6.1`` cannot be loaded simultaneously (as users should never want to have both loaded).

.. TODO Add a 'behind the scenes' on the custom use of own modules
