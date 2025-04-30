:original_name: dws_06_0203.html

.. _dws_06_0203:

DROP ROLE
=========

Function
--------

**DROP ROLE** deletes a specified role.

Precautions
-----------

If a "role is being used by other users" error is displayed when you run **DROP ROLE**, it might be that threads cannot respond to signals in a timely manner during the **CLEAN CONNECTION** process. As a result, connections are not completely cleared. In this case, you need to run **CLEAN CONNECTION** again.

|image1|

-  Be cautious when using **DROP OBJECT** (e.g., **DATABASE**, **USER/ROLE**, **SCHEMA**, **TABLE**, **VIEW**) as it may cause data loss, especially with **CASCADE** deletions. Always back up data before proceeding.
-  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

::

   DROP ROLE [ IF EXISTS ] role_name [, ...];

.. _en-us_topic_0000001811634797__s56aa378dd3884fe5b99b87329c9e93c2:

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified role does not exist.

-  **role_name**

   Specifies the name of the role to be deleted.

   Value range: An existing role.

Examples
--------

Delete the **manager** role:

::

   DROP ROLE manager;

Helpful Links
-------------

:ref:`CREATE ROLE <dws_06_0172>`, :ref:`ALTER ROLE <dws_06_0134>`, :ref:`SET ROLE <dws_06_0222>`

.. |image1| image:: /_static/images/danger_3.0-en-us.png
