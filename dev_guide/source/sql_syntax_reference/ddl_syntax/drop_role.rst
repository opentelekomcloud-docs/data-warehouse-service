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

Syntax
------

::

   DROP ROLE [ IF EXISTS ] role_name [, ...];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified role does not exist.

-  **role_name**

   Specifies the name of the role to be deleted.

   Value range: An existing role.

.. _en-us_topic_0000001145510617__s3ff92f5089cb4291b96221b5f2b85e18:

Examples
--------

Delete the **manager** role.

::

   DROP ROLE manager;

Helpful Links
-------------

:ref:`CREATE ROLE <dws_06_0172>`, :ref:`ALTER ROLE <dws_06_0134>`, :ref:`SET ROLE <dws_06_0222>`
