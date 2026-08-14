:original_name: dws_06_0212.html

.. _dws_06_0212:

DROP TRIGGER
============

Function
--------

**DROP TRIGGER** deletes a trigger.

Precautions
-----------

Only the owner of a trigger and system administrators can run the **DROP TRIGGER** statement.

Syntax
------

::

   DROP TRIGGER [ IF EXISTS ] trigger_name ON table_name [ CASCADE | RESTRICT ];

Parameter Description
---------------------

.. table:: **Table 1** DROP TRIGGER parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                         | Value Range                                                                                      |
   +=======================+=====================================================================================================+==================================================================================================+
   | IF EXISTS             | Sends a notice instead of an error if the specified trigger does not exist.                         | ``-``                                                                                            |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
   | trigger_name          | Specifies the name of the trigger to be deleted.                                                    | An existing trigger.                                                                             |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
   | table_name            | Specifies the name of the table where the trigger to be deleted is located.                         | An existing table having a trigger                                                               |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | Specifies how to process related data in the dependent object when a delete operation is performed. | -  **CASCADE**: Deletes objects that depend on the trigger.                                      |
   |                       |                                                                                                     | -  **RESTRICT**: Refuses to delete the trigger if any objects depend on it. This is the default. |
   +-----------------------+-----------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+

Examples
--------

Delete trigger **insert_trigger**.

::

   DROP TRIGGER IF EXISTS insert_trigger ON test_trigger_src_tbl;

Helpful Links
-------------

:ref:`CREATE TRIGGER <dws_06_0184>`, :ref:`ALTER TRIGGER <dws_06_0147>`, :ref:`ALTER TABLE <dws_06_0142>`
