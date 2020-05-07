.. _git:

Git
===

.. sidebar:: Git

   :URL: https://git-scm.com/

Git 2.17 is available on JADE.  You do not need to load any module files to access it.

Cloning/pushing to/pulling from online Git repositories
-------------------------------------------------------

JADE does not have direct Internet access so you need to tell Git to communicate with GitHub/GitLab via a proxy.

You can configure proxy connections per-repository with:

.. code-block:: sh

   git config http.proxy $http_proxy
   git config https.proxy $https_proxy

Or globally configure the use of a particular proxy with:

.. code-block:: sh

   git config --global http.proxy $http_proxy
   git config --global https.proxy $https_proxy

Then ensure you use ``https://`` rather than ``git://`` when cloning repositories on Jade.
