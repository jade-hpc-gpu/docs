.. _pi-projectmanager:

Information for PIs and Project Managers
========================================

Creating a Project
------------------

To access the JADE HPC facility, please see the information here: `Get access to JADE HPC facilities <http://www.jade.ac.uk/access>`_ 

Getting an Account
------------------

After the project proposal has been approved, a `SAFE <https://um.hartree.stfc.ac.uk>`_ account must be created which allows the management of **users** and **projects**. At registration, the PI provides an institutional email address which will be used as their SAFE login ID (please note, emails such as gmail or hotmail will not be accepted). An SSH Key is also required at this stage, and instructions on how to generate and upload this are provided in `SAFE User Guide <http://community.hartree.stfc.ac.uk/wiki/site/admin/home.html>`_.

Once a project has been set up by Hartree staff, the PI can define associated project managers and then the PI and project managers are able to define groups (in a tree hierarchy), accept people into the project and allocate them to groups.

Projects and Groups on JADE
---------------------------

At the top-level there are **projects** with a PI, and within a **project** there are **groups** with resources such as GPU time and disk space. A **group must be created** in order to accept normal users in to the project.

The simplest approach is to create a single group for the Project for all users, but additional groups can be created if necessary.

**To create a group:**

  1. From the main portal of `SAFE <https://um.hartree.stfc.ac.uk>`_, click the `Administer` button next to the name of the project that you manage.
  2. Click the `Project Group Administration` button, then click `Add New`.
  3. Type in a name and description and click `Create`.

Project Signup Code (instructions for institutional contacts and project managers)
----------------------------------------------------------------------------------

A **project signup code is required** so that users can request to join projects within your institution. This will need to be provided by you to new users within your institution.

Once users have applied with the project code, they will need to be added to the appropriate project group.

**To set a project signup code:**

  1. From the main portal of `SAFE <https://um.hartree.stfc.ac.uk>`_, click the `Administer` button next to the name of the project that you manage.
  2. Click the `Update` button.
  3. In the **Password** field, set the desired **project signup code** and press `Update`.

Adding Users to a Project or group(instructions for institutional contacts and project managers)
------------------------------------------------------------------------------------------------

After creating a group, users can be added to a project.

**To add users:**

  1. Provide your users with the **project name**, **project signup code** (see above section) and signup instruction for regular users at: `http://jade-hpc.readthedocs.io/en/latest/jade/getting-account.html <http://jade-hpc.readthedocs.io/en/latest/jade/getting-account.html>`_
  2. Once a user has requested to join a project, there will be a "New Project Management Requests" box. Click the `Process` button and `Accept` (or `Reject`) the new member.
  3. The PI can now add the member to the a **group** previously created.
  4. Click on `Administer` for the Project then choose `Project Group Administration`.
  5. Click on the `Group name`, then on `Add Account`, and then select any available user from a list (including the PI).
  6. There is now a manual process, which may take up to 24 hours, after which the new Project member will be notified of their new userid and invited to log on to the Hartree systems for the first time.
  7. The new project member will have an `Active` status in the group once the process is completed.

.. note::

  The PI does NOT have to 'Request Join Project' as he/she is automatically a Project member. They must however, add themselves to a Group.


Project groups and shared file system area
------------------------------------------


Each Project group maps to a Unix group, and each Project group member's home file system is set up under a directory group structure. The example below starts from a user's home directory and shows that all other members of the Project group are assigned home directories as "peer" directories. ::

  -bash-4.1$ pwd
  /gpfs/home/training/jpf03/jpf26-jpf03
  -bash-4.1$ cd ..
  -bash-4.1$ ls
  afg27-jpf03  bbl28-jpf03  cxe72-jpf03  dxd46-jpf03  hvs09-jpf03  jjb63-jpf03  jxm09-jpf03  mkk76-jpf03  phw57-jpf03  rrr25-jpf03  rxw47-jpf03  sxl18-jpf03
  ajd95-jpf03  bwm51-jpf03  cxl10-jpf03  dxp21-jpf03  hxo76-jpf03  jkj47-jpf03  kxm85-jpf03  mxm86-jpf03  pxj86-jpf03  rrs70-jpf03  sca58-jpf03  tcn16-jpf03
  axa59-jpf03  bxp59-jpf03  djc87-jpf03  fxb73-jpf03  ivk29-jpf03  jpf26-jpf03  lim17-jpf03  nxt14-jpf03  rja87-jpf03  rwt21-jpf03  shared       txc61-jpf03
  axw52-jpf03  bxv09-jpf03  dwn60-jpf03  gxx38-jpf03  jds89-jpf03  jrh19-jpf03  ltc84-jpf03  pag51-jpf03  rjb98-jpf03  rxl87-jpf03  sls56-jpf03  vvt17-jpf03

Important to some, please note that for each Project group there is a "shared" directory which can be reached at ::

  ../shared

from each user's home directory. Every member of the Project group is able to read and write to this shared directory, so it can be used for common files and applications for the Project.


Once a Project has Finished
---------------------------

It is Hartree Centre policy that, after the agreed date of completion of a Project, all data will be made read-only and will then remain retrievable for 3 months. During this period, users are able to login to retrieve their data, but will be unable to run jobs. After 3 months have elapsed, all login access associated with the Project will be terminated, and all data owned by the Project will be deleted.
