.. _slurm

Slurm
=====

Introduction
------------

Running software on the JADE system is accomplished via batch jobs, *i.e.* in an unattended, non-interactive manner.  Typically a user logs in to the JADE login nodes, prepares a job script and submits it to the job queue.

Jobs on JADE are managed by the Slurm_ batch system, which is in charge of

.. _Slurm: https://slurm.schedmd.com/

* allocating the computer resources requested for the job,
* running the job and
* reporting the outcome of the execution back to the user.

Running a job involves, at the minimum, the following steps

* preparing a submission script and
* submitting the job to execution.

This guide describes basic job submission and monitoring for Slurm.  The topics in the guide are:

* the main Slurm commands,
* preparing a submission script,
* slurm partitions,
* submitting a job to the queue,
* monitoring a job execution,
* deleting a job from the queue and
* environment variables.


Commands
--------
The table below gives a short description of the most used Slurm commands.

+-------------+-------------------------------------------------+
| Command     | Description                                     |
+=============+=================================================+
| ``sacct``   | report job accounting information about active  |
|             | or completed jobs                               |
+-------------+-------------------------------------------------+
| ``salloc``  | allocate resources for a job in real time       |
|             | (typically used to allocate resources and       |
|             | spawn a shell, in which the srun command is     |
|             | used to launch parallel tasks)                  |
+-------------+-------------------------------------------------+
| ``sbatch``  | submit a job script for later execution         |
|             | (the script typically contains one or more      |
|             | ``srun`` commands to launch parallel tasks)     |
+-------------+-------------------------------------------------+
| ``scancel`` | cancel a pending or running job                 |
+-------------+-------------------------------------------------+
| ``sinfo``   | reports the state of partitions and nodes       |
|             | managed by Slurm (it has a variety of           |
|             | filtering, sorting, and formatting options)     |
+-------------+-------------------------------------------------+
| ``squeue``  | reports the state of jobs (it has a variety of  |
|             | filtering, sorting, and formatting options),    |
|             | by default, reports the running jobs in         |
|             | priority order followed by the pending jobs in  |
|             | priority order                                  |
+-------------+-------------------------------------------------+
| ``srun``    | used to submit a job for execution in real time |
+-------------+-------------------------------------------------+

All Slurm commands have extensive help through their man pages *e.g.*::

  man sbatch

shows you the help pages for the ``sbatch`` command.


Preparing a submission script
-----------------------------

A submission script is a Linux shell script that

* describes the processing to carry out (e.g. the application, its input and output, etc.) and
* requests computer resources (number of cpus, amount of memory, etc.) to use for processing.

The simplest case is that of a job that requires a single node (this is the smallest unit we allocate on arcus-b) with the following requirements:

* the job uses 1 node,
* the application is a single process,
* the job uses a single GPU,
* the job will run for no more than 10 hours,
* the job is given the name "job123" and
* the user should be emailed when the job starts and stops or aborts.

Supposing the application run is called ``myCode`` and takes no command line arguments, the following submission script runs the application in a single job:::

  #!/bin/bash

  # set the number of nodes
  #SBATCH --nodes=1

  # set max wallclock time
  #SBATCH --time=10:00:00

  # set name of job
  #SBATCH --job-name=job123

  # set number of GPUs
  #SBATCH --gres=gpu:1

  # mail alert at start, end and abortion of execution
  #SBATCH --mail-type=ALL

  # send mail to this address
  #SBATCH --mail-user=john.brown@gmail.com

  # run the application
  myCode

The script starts with ``#!/bin/bash`` (also called a shebang), which makes the submission script a Linux bash script.

The script continues with a series of lines starting with ``#``, which represent bash script comments.  For Slurm, the lines starting with ``#SBATCH`` are directives that request job scheduling resources.  (Note: it is important that you put all the directives at the top of a script, before any other commands; any ``#SBATCH`` directive coming after a bash script command is ignored!)

The resource request ``#SBATCH --nodes=n`` determines how many compute nodes a job are allocated by the scheduler; only 1 node is allocated for this job.

The maximum walltime is specified by ``#SBATCH --time=T``, where ``T`` has format **h:m:s**.  Normally, a job is expected to finish before the specified maximum walltime.  After the walltime reaches the maximum, the job terminates regardless whether the job processes are still running or not. 

The name of the job can be specified too with ``#SBATCH --job-name=name``.

Lastly, an email notification is sent if an address is specified with ``#SBATCH --mail-user=<email_address>``.  The notification options can be set with ``#SBATCH --mail-type=<type>``, where ``<type>`` may be ``BEGIN``, ``END``, ``FAIL``, ``REQUEUE`` or ``ALL`` (for any change of job state).

The final part of a script is normal Linux bash script and describes the set of operations to follow as part of the job.  The job starts in the same folder where it was submitted (unless an alternative path is specified), and with the same environment variables (modules, etc.) that the user had at the time of the submission.  In this example, this final part only involves invoking the ``myCode`` application executable.
