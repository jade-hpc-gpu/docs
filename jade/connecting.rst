.. _connecting:

Connecting to the cluster using SSH
===================================

To log onto the JADE cluster you must use `SSH <https://en.wikipedia.org/wiki/Secure_Shell>`_, which is a common way of remotely logging in to computers running the GNU/Linux operating system.

To do this, you need to have a SSH *client* program installed on your machine. macOS and GNU/Linux come with a command-line (text-only) SSH client pre-installed.  On Windows there are various graphical SSH clients you can use, including *MobaXTerm*.


SSH client software on Windows
------------------------------

Download and install the *Installer edition* of `mobaxterm <https://mobaxterm.mobatek.net/download-home-edition.html>`_.

After starting MobaXterm you should see something like this:

.. image:: /images/mobaxterm-welcome.png
   :width: 50%
   :align: center

Click *Start local terminal* and if you see something like the following then please continue to :ref:`ssh-connection`.

.. image:: /images/mobaxterm-terminal.png
   :width: 50%
   :align: center

Running commands from a terminal (from the command-line) may initially be
unfamiliar to Windows users but this is the recommended approach for
running commands on ShARC and Iceberg as
it is the idiomatic way of interfacing with the GNU/Linux clusters.

SSH client software on Mac OS/X and GNU/Linux
-----------------------------------------

GNU/Linux and macOS (OS X) both typically come with a command-line SSH client pre-installed.

If you are using macOS and want to be able to run graphical applications on the clusters then
you need to install the latest version of the `XQuartz <https://www.xquartz.org/>`_ *X Windows server*.

Open a terminal (e.g. *Gnome Terminal* on GNU/Linux or *Terminal* on macOS) and then go to :ref:`ssh-connection`.

.. _ssh-connection:

Establishing a SSH connection
-----------------------------

Once you have a terminal open, run the following command to log into one of the JADE front-end nodes: ::

  ssh -l $USER jade.hartree.stfc.ac.uk

Here you need to replace ``$USER`` with your username (e.g. ``te1st-test``)

.. note::

    **macOS users**: if this fails then:

    * Check that your `XQuartz <https://www.xquartz.org/>`_ is up to date then try again *or*
    * Try again with ``-Y`` instead of ``-X``


.. note::

   JADE has multiple front-end systems, and because of this some SSH software operating under stringent security settings might give warnings about possible man-in-the-middle attacks because of apparent changes in machine settings. This is a known issue and is being addressed, but in the meantime these warnings can be safely ignored.


.. note::

    When you login to a cluster you reach one of two login nodes.
    You **should not** run applications on the login nodes.
    Running ``srun`` gives you an interactive terminal
    on one of the many worker nodes in the cluster.
