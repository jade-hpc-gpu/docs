.. _getting-account:

Getting an account
==================

As a regular user, getting started involves 3 steps:

1) Apply for a Hartree SAFE account
-----------------------------------

This is a web account which will show you which projects you belong to, and the accounts which you have in them.

Before applying for a SAFE account, you should first have an SSH key-pair, and be ready to provide your public key as part of the SAFE registration process. See :ref:`connecting-generate-ssh-keys` or contact your local IT support staff if you need further assistance.

Once you have your public SSH key ready, apply for your SAFE account by going here:
https://um.hartree.stfc.ac.uk/hartree/login.jsp
and providing all of the required information.

When your account has been approved, you will receive an email giving your initial password. When you login for the first time you will be asked to change it to a new one.

2) Apply for a JADE project account
-----------------------------------

Once your SAFE account is established, login to it and click on "Request Join Project".

From the drop-down list select the appropriate **project**, enter the **signup code** which you should have been given by the project PI or manager, and then click "Request".

The approval process goes through several steps:
  a) approval by the PI or project manager -- once this is done the SAFE status changes to Pending
  b) initial account setup --  once this is done the SAFE status changes to Active
  c) completion of account setup -- once this is done you will get an email confirming you are all set, and your SAFE account will have full details on your new project account

This process shouldn't take more than 2 working days.  If it takes more than that, check whether the PI or project manager is aware that you have applied, and therefore your application needs their approval through the SAFE system.

If your SAFE userid is xyz, and your project suffix is abc, then your project account username will be xyz-abc and you will login to JADE using the command:

ssh -l xyz-abc jade2.hartree.stfc.ac.uk

Note that some users may belong to more than one project, in which case they will have different account usernames for each project, and all of them will be listed on their SAFE web account.

Each project account will have a separate file structure, and separate quotas for GPU time, filestore and other system resources.

Note also that JADE has multiple front-end systems, and because of this some SSH software operating under stringent security settings might give warnings about possible man-in-the-middle attacks because of apparent changes in machine settings.  This is a known issue and is being addressed, but in the meantime these warnings can be safely ignored.

3) Apply for a Hartree ServiceNow account
-----------------------------------------

This is a web account used for reporting any operational issues with JADE. The account has to be created separately from your SAFE account.

ServiceNow can be found at `https://stfc.service-now.com/hcssp <https://stfc.service-now.com/hcssp>`_. Click on the `register <https://stfc.service-now.com/hcssp?id=csm_registration>`_ link to create an account. After the approval you will be able to access the Service Now portal.
