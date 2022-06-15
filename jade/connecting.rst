.. _connecting:

Connecting to the cluster using SSH
===================================

To log onto the JADE cluster you must use `SSH <https://en.wikipedia.org/wiki/Secure_Shell>`_, which is a common way of remotely logging in to computers running the GNU/Linux operating system.

To do this, you need to have a SSH *client* program installed on your machine. macOS and GNU/Linux come with a command-line (text-only) SSH client pre-installed.  On Windows there are various graphical SSH clients you can use, including *MobaXTerm*.

.. _connecting-ssh-client-windows:

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
running commands on JADE as
it is the idiomatic way of interfacing with the GNU/Linux clusters.

SSH client software on Mac OS/X and GNU/Linux
---------------------------------------------

GNU/Linux and macOS (OS X) both typically come with a command-line SSH client pre-installed.

If you are using macOS and want to be able to run graphical applications on the clusters then
you need to install the latest version of the `XQuartz <https://www.xquartz.org/>`_ *X Windows server*.

Open a terminal (e.g. *Gnome Terminal* on GNU/Linux or *Terminal* on macOS) and then go to :ref:`ssh-connection`.

.. _connecting-generate-ssh-keys:

Generating SSH Keys
-------------------

SSH keys allows you to connect remotely to JADE through SSH without the need for passwords. When generating
SSH keys, a pair of public (normally with .pub extension) and private keys are generated.

You will need to provide your SSH public key as part of your SAFE account registration process. Your private key
should never be shared with anybody else and will be used by your SSH client when connecting to JADE  (e.g. ssh on the
command line or MobaXterm on Windows).

#. Open your preferred terminal/shell/command line app (for Windows, see :ref:`connecting-ssh-client-windows`).
#. Type in: ::

    ssh-keygen -t rsa

#. You will be asked where you'd like to save the key. Just pressing enter will save to a default location at ``/home/[yourusername]/.ssh/id_rsa``).
#. You will then be asked for a passphrase of the key. Just pressing enter will skip the use of passphrase but it's recommended to set one).
#. You will have to confirm your passphrase, or press enter again if a passphrase is not set.
#.  Your key is now generated and written to the location you specified, notice that there's two files, a private key without an
    extension (e.g. ``id_rsa``) and public key with a ``.pub`` extension (e.g. ``id_rsa.pub``)
#. When registering for a SAFE account, enter the content of your public key file (.pub extension) when asked.

An example of the output can be seen below: ::

    $ ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/my_username/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/my_username/.ssh/id_rsa
    Your public key has been saved in /home/my_username/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:weZ3rpaY5kV0OKX8Z6hTlxpAso6ZOaARtpgUYHk18rI my_username@my_machine_name
    The key's randomart image is:
    +---[RSA 3072]----+
    |o+=..o  . . .    |
    |o= +o .. = +     |
    |o +...  = B .    |
    |   oo. O o = . . |
    |  .E  * S o * =  |
    |       . o = *   |
    |         o+.o    |
    |        +.oo     |
    |       o...      |
    +----[SHA256]-----+

.. note::
    MobaXterm has its own ``home`` directory path which can be opened using the command ``open /home/mobaxterm``. You can then place key files generated externally into the ``.ssh`` folder.

.. _ssh-connection:

Establishing a SSH connection
-----------------------------

Once you have a terminal open, run the following command to log into one of the JADE front-end nodes: ::

  ssh -l $USER jade2.hartree.stfc.ac.uk

Here you need to replace ``$USER`` with your username (e.g. ``te1st-test``)


.. note::

    If you have key files named ``id_rsa`` your home's ``.ssh`` directory, it will be used by default. You can manually specify a key's path by adding an ``-i`` flag if your key has a different name or is in a different location e.g.

        ``ssh -l $USER -i /home/my_username/.ssh/my_custom_key jade2.hartree.stfc.ac.uk``

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
