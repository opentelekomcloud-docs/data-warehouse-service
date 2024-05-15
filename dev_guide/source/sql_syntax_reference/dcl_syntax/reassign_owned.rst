:original_name: dws_06_0252.html

.. _dws_06_0252:

REASSIGN OWNED
==============

Function
--------

**REASSIGN OWNED** changes the owner of a database.

**REASSIGN OWNED** requires that the system change owners of all the database objects owned by **old_role** to **new_role**.

Precautions
-----------

-  **REASSIGN OWNED** is often executed before deleting a rule. Because objects in other databases are not affected, you usually need to run this command in each database that contains the objects owned by the role to be deleted.
-  You must have the permissions on the original and target roles to execute it.
-  The resource management module does not monitor the data switch of the syntax. You need to call **select gs_wlm_readjust_user_space(0)** to manually calibrate the monitoring data.

Syntax
------

::

   REASSIGN OWNED BY old_role [, ...] TO new_role;

Parameter Description
---------------------

-  **old_role**

   Specifies the role name of the old owner.

-  **new_role**

   Specifies the role name of the new owner.

Examples
--------

Create two users:

::

   CREATE USER joe PASSWORD '{Password}';
   CREATE USER jack PASSWORD '{Password}';

Reassign all database objects owned by the **joe** and **jack** roles to **admin**:

::

   REASSIGN OWNED BY joe, jack TO dbadmin;
