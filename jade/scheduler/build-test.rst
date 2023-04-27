.. _buildtest:

The BUILD TEST FUBAR
====================

Introduction
------------

I've been added into a subfolder

Running software on the JADE system is accomplished via batch jobs, *i.e.* in an unattended, non-interactive manner.  Typically a user logs in to the JADE login nodes, prepares a job script and submits it to the job queue.

Jobs on JADE are managed by the `Slurm batch system <https://slurm.schedmd.com>`_ , which is in charge of:

* allocating the computer resources requested for the job,
* running the job and
* reporting the outcome of the execution back to the user.

Running a job involves, at the minimum, the following steps

* preparing a submission script and
* submitting the job to execution.

This guide describes basic job submission and monitoring for Slurm.  The generic topics in the guide are:

* the main Slurm commands,
* preparing a submission script,
* submitting a job to the queue,
* monitoring a job execution,
* deleting a job from the queue and
* important environment variables.

Additionally, the following topics specific to JADE are covered (*under construction*)

* slurm partitions,
* using fast local storage and
* controlling affinity.


Commands
--------
The table below gives a short description of the Slurm commands that are likely to be useful to most users.

+-------------+-------------------------------------------------+
| Command     | Description                                     |
+=============+=================================================+
| ``sacct``   | report job accounting information about active  |
|             | or completed jobs                               |
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

All Slurm commands have extensive help through their man pages *e.g.*::

  man sbatch

shows you the help pages for the ``sbatch`` command.

In addition to the above commands, the table below gives two more commands that can be used in special cases, *e.g.* to obtain an interactive session, such as used in the Machine Learning examples.  The commands are

+-------------+-------------------------------------------------+
| Command     | Description                                     |
+=============+=================================================+
| ``salloc``  | allocate resources for a job in real time       |
|             | (typically used to allocate resources and       |
|             | spawn a shell, in which the srun command is     |
|             | used to launch parallel tasks)                  |
+-------------+-------------------------------------------------+
| ``srun``    | used to submit a job for execution in real time |
+-------------+-------------------------------------------------+

*N.B.* ``srun`` can be used to launch application into execution from within submission scripts.  The success of this in the case of MPI distributed applications depends on the MPI software stack having been build with support for PMI (Process Management Interface).



Preparing a submission script
-----------------------------

A submission script is a shell script that

* describes the processing to carry out (*e.g.* the application, its input and output, etc.) and
* requests computer resources (number of GPUs, amount of memory, etc.) to use for processing.

The simplest case is that of a job that requires a single node with the following requirements:

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

The script starts with ``#!/bin/bash`` (also called a shebang), which makes the submission script a GNU bash script.

The script continues with a series of lines starting with ``#``, which represent bash script comments.  For Slurm, the lines starting with ``#SBATCH`` are directives that request job scheduling resources.  (Note: it is important that you put all the directives at the top of a script, before any other commands; any ``#SBATCH`` directive coming after a bash script command is ignored!)

The resource request ``#SBATCH --nodes=n`` determines how many compute nodes a job are allocated by the scheduler; only 1 node is allocated for this job.

The maximum walltime is specified by ``#SBATCH --time=T``, where ``T`` has format ``H:M:S``.  Normally, a job is expected to finish before the specified maximum walltime.  After the walltime reaches the maximum, the job terminates regardless whether the job processes are still running or not.

The name the job is identified by in the queue can be specified too with ``#SBATCH --job-name=name``.

Lastly, an email notification is sent if an address is specified with ``#SBATCH --mail-user=<email_address>``.  The notification options can be set with ``#SBATCH --mail-type=<type>``, where ``<type>`` may be ``BEGIN``, ``END``, ``FAIL``, ``REQUEUE`` or ``ALL`` (for any change of job state).

The final part of a script is normal GNU bash script and describes the set of operations to follow as part of the job.  The job starts in the same folder where it was submitted (unless an alternative path is specified), and with the same environment variables (modules, etc.) that the user had at the time of the submission.  In this example, this final part only involves invoking the ``myCode`` application executable.


Submitting jobs with the command sbatch
---------------------------------------

Once you have a submission script ready (*e.g* called ``submit.sh``), the job is submitted to the execution queue with the command ``sbatch script.sh``.  The queueing system prints a number (the job id) almost immediately and returns control to the linux prompt.  At this point the job is in the submission queue.

Once the job submitted, it will sit in a pending state until the resources have been allocated to your job (the length of time your job is in the pending state is dependent upon a number of factors including how busy the system is and what resources you are requesting). You can monitor the progress of the job using the command ``squeue`` (see below).

Once the job starts to run you will see files with names such as ``slurm-1234.out`` either in the directory you submitted the job from (default behaviour) or in the directory where the script was instructed explicitly to change to.

Job partitions on JADE
----------------------

Partitions are Slurm entities defined by the system administrators that allow the separation and control of jobs according to their characteristics.  Each partition has a a number of compute nodes associated with it, as well as properties that control job placement.  A job can be submitted to be executed by a particular partition, and if no partition is specified, the default one is selected.

There are three partitions on JADE, which are:

+----------------+--------------------------------------+
| Partition name | Description                          |
+================+======================================+
| ``big``        | Partition dedicated to jobs that     |
|                | occupy an entire node, *i.e.* 8 GPUs |
+----------------+--------------------------------------+
| ``small``      | Partition dedicated to jobs that     |
|                | utilise a single GPUs each.          |
+----------------+--------------------------------------+
| ``devel``      | Partition dedicated to testing.      |
+----------------+--------------------------------------+

The partitions have the following limits for submitted jobs:

+----------------+---------------------+-------------------+-------------------+
| Partition name | Partition Size      |Job Walltime limit | Running Job limit |
+================+=====================+===================+===================+
| ``big``        | 11 nodes            | 24 hours          | 5 Jobs            |
|                |                     |                   |                   |
+----------------+---------------------+-------------------+-------------------+
| ``small``      | 10 nodes            | 6 days            | 16 Jobs           |
|                |                     |                   |                   |
+----------------+---------------------+-------------------+-------------------+
| ``devel``      | 1 node              | 1 hour            | 1 Job             |
+----------------+---------------------+-------------------+-------------------+


The default partition is ``big``.  Information on these partitions can be obtained with the commands ``sinfo -a`` or ``scontrol show partition=small``.

Submitting to a particular partition can be done by specifying the partition as an argument to ``sbatch``, *e.g.* ``sbatch -p devel sub.sh``, or by directly supplying a request for that partition in the submission script, *e.g.* ``#SBATCH --partition=devel``.

The ``devel`` partition should be used to check your submission script works correctly and that your application starts to execute without errors.

Upon reaching the per user running job limit for a partition, any further jobs submitted to that same partition by the same user will be shown as state Pending (PD) with the Reason set as QOSMaxJobsPerUserLimit.


Monitoring jobs with the command squeue
---------------------------------------

``squeue`` is the main command for monitoring the state of systems, groups of jobs or individual jobs.

The command ``squeue`` prints the list of current jobs.  The list looks something like this: ::

  | JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
  |  2497     devel     srun      bob  R       0:07      1 dgj416
  |  2499       big     test1    mary  R       0:22      4 dgj[101,104]
  |  2511     small     test2   steve PD       0:00      4 (Resources)

The first column gives the job ID, the second the partition where the job was submitted, the third the name of the job (specified by the user in the submission script) and the fourth the user ID of the job owner.  The fifth is the status of the job (**R** = running, **PD** = pending, **CA** = cancelled, **CF** = configuring, **CG** = completing, **CD** = completed, **F** = failed). The sixth column gives the elapsed time for each particular job.  Finally, there are the number of nodes requested and the nodelist where the job is running (or the cause that it is not running).

Some useful command line options for ``squeue`` include:

* ``-u`` for showing the status of all the jobs of a particular user, *e.g.* ``squeue -u bob``;
* ``-l`` for showing more of the available information;
* ``-j`` for showing information regarding a particular job ID, *e.g.*  ``squeue -j 7890``;
* ``--start`` to report  the  expected  start  time  of pending jobs.

Read all the options for squeue using the command ``man squeue``, including how to personalize the information to be displayed.


Deleting jobs with the command scancel
--------------------------------------

Use the ``scancel`` command to delete a job, *e.g.* ``scancel 1121`` to delete job with ID **1121**.  Any user can delete their own jobs at any time, whether the job is pending (waiting in the queue) or running.  A user cannot delete the jobs of another user.  Normally, there is a (small) delay between the execution of the ``scancel`` command and the time when the job is dequeued and killed.


Environment variables
---------------------

At the time a job is launched into execution, Slurm defines multiple environment variables, which can be used from within the submission script to define the correct workflow of the job.  A few useful environment variables are the following:

* ``SLURM_SUBMIT_DIR``, which points to the directory where the sbatch command is issued;
* ``SLURM_JOB_NODELIST``, which returns the list of nodes allocated to the job;
* ``SLURM_JOB_ID``, which is a unique number Slurm assigns to a job.

In most cases, ``SLURM_SUBMIT_DIR`` does not have to be used, as the job lands by default in the directory where the Slurm command ``sbatch`` was issued.

``SLURM_SUBMIT_DIR`` can be useful in a submission script when files must be copied to/from a specific directory that is different from the directory where ``sbatch`` was issued.

``SLURM_JOB_ID`` is useful to tag job specific files and directories (typically output files or run directories) in order to identify them as produced by a particular job.  For instance, the submission script line ::

  myApp &> $SLURM_JOB_ID.out

runs the application myApp and redirects the standard output (and error) to a file whose name is given by the job ID.  *Note*: the job ID is a number assigned by Slurm and differs from the character string name given to the job in the submission script by the user.


Job arrays
----------
Job arrays is a useful mechanism for submitting and managing collections of similar jobs quickly and easily; multiple job are submitted to the queue using a single ``sbatch`` command and a single submission script.

Here are a few examples:::

  # submit a job array with index values between 0 and 7
  $ sbatch --array=0-7 sub.sh

  # submit a job array with index values of 1, 3, 5 and 7
  $ sbatch --array=1,3,5,7 sub.sh

  # submit a job array with index values between 1 and 7 with a step size of 2 (i.e. 1, 3, 5 and 7)
  $ sbatch --array=1-7:2 sub.sh

The index values are used by Slurm to initialise two environment variables when the job launches into execution.  These variables are

* ``SLURM_ARRAY_JOB_ID``, set to the first job ID of the array and
* ``SLURM_ARRAY_TASK_ID``, set to the job array index value.

To give an example, suppose you submit an array of three jobs using the submission command ``sbatch --array=1-3 sub.sh``, which returns::

  Submitted batch job 10

Then, the environment variables in the three jobs will be

+------------------+------------------------+
| Job array index  | Variables              |
+==================+========================+
| 1                | SLURM_ARRAY_JOB_ID=10; |
|                  | SLURM_ARRAY_TASK_ID=1  |
+------------------+------------------------+
| 2                | SLURM_ARRAY_JOB_ID=10; |
|                  | SLURM_ARRAY_TASK_ID=2  |
+------------------+------------------------+
| 3                | SLURM_ARRAY_JOB_ID=10; |
|                  | SLURM_ARRAY_TASK_ID=3  |
+------------------+------------------------+

The above environment variables can be used within the submission script to define what each individual job within the array does.  To take a simple example, suppose each job in the array uses a single GPU and takes the input from a file that is identified by the same index as the job.  The submission script could look like this::

  #!/bin/bash

  #SBATCH --nodes=1
  #SBATCH --job-name=test
  #SBATCH --time=00:30:00
  #SBATCH --gres=gpu:1

  myCode --input "file_${SLURM_ARRAY_TASK_ID}.inp"

To reiterate, the advantage of using job arrays is a single job script as the one above can be used to launch a large number of jobs, each working on a different tasks, in a controlled way.
