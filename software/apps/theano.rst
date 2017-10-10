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


Using Theano in Batch Mode
------------------------------




Reserving Memory to Improve Theano Performance
----------------------------------------------

For optimal Theano performance, enable the CUDA memory manager CNMeM. To do this, create the .theanorc file in your HOME directory and set the fraction of GPU memory reserved by Theano. The exact amount of energy may have to be hand-picked: if Theano asks for more memory that is currently available on the GPU, an error will be thrown during import of theano module. Create or edit the ``.theanorc`` file with nano: ::

		nano ~/.theanorc

Add the following lines and, if necessary, change the 0.8 number to whatever works for you ::

		[lib]
		cnmem=0.8

Run python and verify that Theano is working correctly ::

		python -c "import theano;theano.test()"
