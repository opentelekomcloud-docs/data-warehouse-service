:original_name: dws_06_0214.html

.. _dws_06_0214:

DROP USER
=========

Function
--------

Deleting a user will also delete the schema having the same name as the user.

Precautions
-----------

-  **CASCADE** is used to delete objects (excluding databases) that depend on the user. **CASCADE** cannot delete locked objects unless the locked objects are unlocked or the processes that lock the objects are stopped.
-  When deleting a user in the database, if the object that the user depends on is in another database or the object of the dependent user is another database, you need to manually delete the dependent objects in other databases or delete the dependent database. Then, delete the user. Cross-database cascading deletion cannot be performed.
-  In a multi-tenant scenario, the service user will also be deleted when you delete a user group. If the specified **CASCADE** concatenation is deleted, **CASCADE** will be specified upon the deletion of the service user. If you fail to delete a user, an error is reported, and you cannot delete other users either.
-  If the error table specified by the GDS foreign table created by user A is under the schema of user B, user B cannot be deleted by specifying the **CASCADE** keyword in **DROP USER**.
-  If a "role is being used by other users" error is displayed when you run **DROP USER**, it might be that threads cannot respond to signals in a timely manner during the **CLEAN CONNECTION** process. As a result, connections are not completely cleared. In this case, you need to run **CLEAN CONNECTION** again.

|image1|

-  Be cautious when using **DROP OBJECT** (e.g., **DATABASE**, **USER/ROLE**, **SCHEMA**, **TABLE**, **VIEW**) as it may cause data loss, especially with **CASCADE** deletions. Always back up data before proceeding.
-  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

.. code-block::

   DROP USER [ IF EXISTS ] user_name [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified user does not exist.

-  **user_name**

   Specifies the name of a user to be deleted.

   Value range: An existing user name.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes all objects (such as tables) that depend on the user to be deleted. When a user is deleted in CASCADE mode, objects owner by the user and the user's permissions for objects will be deleted.
   -  **RESTRICT**: refuses to delete the user if any objects depend on it. This is the default.

   .. note::

      In GaussDB(DWS), the **postgresql.conf** file contains the **enable_kill_query** parameter. This parameter affects the action of deleting user objects using **CASCADE**.

      -  If **enable_kill_query** is **on** and **CASCADE** is used to delete user objects, the processes will be automatically killed and the user will be deleted at the same time.
      -  If **enable_kill_query** is **off** and **CASCADE** is used to delete user objects, the user will be deleted after the processes are automatically killed.

Example
-------

Delete user **jim**:

::

   DROP USER jim CASCADE;

Links
-----

:ref:`ALTER USER <dws_06_0149>`, :ref:`CREATE USER <dws_06_0186>`

.. |image1| image:: /_static/images/danger_3.0-en-us.png
