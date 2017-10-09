.. _connecting:

Connecting to the cluster using SSH
===================================

The most versatile way to **run commands and submit jobs** on the cluster is to
use a mechanism called `SSH <https://en.wikipedia.org/wiki/Secure_Shell>`__,
which is a common way of remotely logging in to computers
running the Linux operating system.

To connect to another machine using SSH you need to
have a SSH *client* program installed on your machine.
macOS and Linux come with a command-line (text-only) SSH client pre-installed.
On Windows there are various graphical SSH clients you can use,
including *MobaXTerm*.


SSH client software on Windows
------------------------------

Download and install the *Installer edition* of `mobaxterm <https://mobaxterm.mobatek.net/download-home-edition.html>`_.

After starting MobaXterm you should see something like this:

.. image:: /images/mobaxterm-welcome.png
   :width: 50%
   :align: center

Click *Start local terminal* and if you see something like the following then please continue to :ref:`ssh`.

.. image:: /images/mobaxterm-terminal.png
   :width: 50%
   :align: center

Running commands from a terminal (from the command-line) may initially be
unfamiliar to Windows users but this is the recommended approach for
running commands on ShARC and Iceberg as
it is the idiomatic way of interfacing with the Linux clusters.

SSH client software on Mac OS/X and Linux
-----------------------------------------

Linux and macOS (OS X) both typically come with a command-line SSH client pre-installed.

If you are using macOS and want to be able to run graphical applications on the clusters then
you need to install the latest version of the `XQuartz <https://www.xquartz.org/>`_ *X Windows server*.

Open a terminal (e.g. *Gnome Terminal* on Linux or *Terminal* on macOS) and then go to :ref:`ssh`.

.. _ssh:

Establishing a SSH connection
-----------------------------

Once you have a terminal open run the following command to
log in to a cluster: ::

    ssh -l $USER jade.hartree.stfc.ac.uk

Here you need to replace ``$USER`` with your username (e.g. ``te1st-test``)

.. note::

    **macOS users**: if this fails then:

    * Check that your `XQuartz <https://www.xquartz.org/>`_ is up to date then try again *or*
    * Try again with ``-Y`` instead of ``-X``

This should give you a prompt resembling the one below: ::

    te1st-test@dgj223:~$
