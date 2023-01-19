:original_name: dws_04_0064.html

.. _dws_04_0064:

Setting Account Security Policies
=================================

Background
----------

For data security purposes, GaussDB(DWS) provides a series of security measures, such as automatically locking and unlocking accounts, manually locking and unlocking abnormal accounts, and deleting accounts that are no longer used.

Automatically Locking and Unlocking Accounts
--------------------------------------------

-  If a user fails to enter the correct password for over 10 times during database connection, the system automatically locks the account.
-  An account is automatically unlocked one day after it was locked.

Manually Locking and Unlocking Accounts
---------------------------------------

If administrators detect an abnormal account that may be stolen or illegally accesses the database, they can manually lock the account.

The administrator can also manually unlock the account if the account becomes normal again.

For details about how to create a user, see :ref:`Users <dws_04_0057>`. To manually lock and unlock user **joe**, run commands in the following format:

-  To manually lock the user:

   ::

      ALTER USER joe ACCOUNT LOCK;

-  To manually unlock the user:

   ::

      ALTER USER joe ACCOUNT UNLOCK;

Deleting Accounts that Are No Longer Used
-----------------------------------------

An administrator can delete an account that is no longer used. This operation cannot be rolled back.

When an account to be deleted is in the active state, it is deleted after the session is disconnected.

For example, if you want to delete account **joe**, run the command in the following format:

::

   DROP USER joe CASCADE;
