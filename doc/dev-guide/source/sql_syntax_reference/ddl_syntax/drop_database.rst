:original_name: dws_06_0189.html

.. _dws_06_0189:

DROP DATABASE
=============

Function
--------

**DROP DATABASE** deletes a database.

Important Notes
---------------

-  Only the owner of a database or a system administrator has the permission to run the **DROP DATABASE** command.
-  **DROP DATABASE** does not take effect for the three preinstalled system databases (**gaussdb**, **TEMPLATE0**, and **TEMPLATE1**) because they are protected. To check databases in the current service, run the **\\l** command of **gsql**.
-  This command cannot be run while the database to be deleted is associated with a user. You can check the current database connections in the **v$session** view.
-  **DROP DATABASE** cannot be run inside a transaction block.
-  If **DROP DATABASE** fails to be run and is rolled back, run **DROP DATABASE IF EXISTS**.
-  If a "database is being accessed by other users" error is displayed when you run **DROP DATABASE**, it might be that threads cannot respond to signals in a timely manner during the **CLEAN CONNECTION** process. As a result, connections are not completely cleared. In this case, you need to run **CLEAN CONNECTION** again.

|image1|

-  Be cautious when using **DROP OBJECT** (e.g., **DATABASE**, **USER/ROLE**, **SCHEMA**, **TABLE**, **VIEW**) as it may cause data loss, especially with **CASCADE** deletions. Always back up data before proceeding.
-  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

::

   DROP DATABASE [ IF EXISTS ] database_name ;

Parameters
----------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified database does not exist.

-  **database_name**

   Specifies the name of the database to be deleted.

   Value range: A string indicating an existing database name.

Examples
--------

Delete the database named **music**.

::

   DROP DATABASE music;

Links
-----

:ref:`CREATE DATABASE <dws_06_0156>`, :ref:`ALTER DATABASE <dws_06_0120>`

.. |image1| image:: /_static/images/danger_3.0-en-us.png
