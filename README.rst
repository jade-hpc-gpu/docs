.. image:: https://travis-ci.org/jade-hpc-gpu/docs.svg?branch=master
    :target: https://travis-ci.org/jade-hpc-gpu/docs

.. image:: https://readthedocs.org/projects/jade-hpc/badge/?version=latest
:target: http://jade-hpc.readthedocs.io/en/latest/?badge=latest
:alt: Documentation Status

JADE HPC Facility Documentation
===============================
This is the source code for the documentation of JADE HPC facility user guide. It is written in the rst format. For a guide on the rst file format see `this <http://thomas-cokelaer.info/tutorials/sphinx/rest_syntax.html>`_ document.


How to Contribute
-----------------
To contribute to this documentation, first you have to fork it on GitHub and clone it to your machine, see `Fork a Repo <https://help.github.com/articles/fork-a-repo/>`_ for the GitHub documentation on this process.

Once you have the git repository locally on your computer, you will need to install ``sphinx`` and ``sphinx_bootstrap_theme`` to be able to build the documentation. See the instructions below for how to achieve this.

::

	pip install sphinx_rtd_theme

Once you have made your changes and updated your Fork on GitHub you will need to `Open a Pull Request <https://help.github.com/articles/using-pull-requests/>`_. All changes to the repository should be made through Pull Requests, including those made by the people with direct push access.


Building the documentation
##########################

#. Install Python on your machine

#. Install sphinx: ::

	pip install sphinx sphinx_rtd_theme

#. To build the HTML documentation run: ::

    make html

   Or if you don't have the ``make`` utility installed on your machine then build with *sphinx* directly: ::

    sphinx-build . ./html



#. Build the documentation: ::

     make html

   Or if you don't have the ``make`` utility installed on your machine then serve with *sphinx* directly: ::

    sphinx-build . ./html


Continuous build and serve
##########################

The package `sphinx-autobuild <https://github.com/GaretJax/sphinx-autobuild>`_ provides a watcher that automatically rebuilds the site as files are modified. To use it, install (in addition to the Sphinx packages) with the following: ::

    pip install sphinx-autobuild

To start the autobuild process, run: ::

    sphinx-autobuild . ./html

The application also serves up the site at port ``8000`` by default at http://localhost:8000.


Making Changes to the Documentation
-----------------------------------

The documentation consists of a series of `reStructured Text <http://sphinx-doc.org/rest.html>`_ files which have the ``.rst`` extension. These files are then automatically converted to HTMl and combined into the web version of the documentation by sphinx. It is important that when editing the files the syntax of the rst files is followed.


If there are any errors in your changes the build will fail and the documentation  will not update, you can test your build locally by running ``make html``. The easiest way to learn what files should look like is to read the ``rst`` files already in the repository.

Submitting Changes and Making Contributions
-------------------------------------------

Contributions should be made by forking the documentation site repo (this repo) and submitting a pull request. Pull requests will be merged by an Admin after review.
