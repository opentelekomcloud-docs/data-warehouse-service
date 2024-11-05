:original_name: dws_06_0125.html

.. _dws_06_0125:

ALTER FOREIGN TABLE (SQL on other GaussDB(DWS))
===============================================

Function
--------

Modifies a foreign table in associated analysis.

Precautions
-----------

None

Syntax
------

-  Set a foreign table's attributes.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          OPTIONS ( {[ SET ] option ['value']} [, ... ]);

-  Set the owner of a foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          OWNER TO new_owner;

-  Update the type of a foreign table column.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] table_name
          MODIFY ( { column_name data_type [, ...] } );

-  Modify a column of a foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          action [, ... ];

   The **action** syntax is as follows:

   ::

      ALTER [ COLUMN ] column_name [ SET DATA ] TYPE data_type
          | MODIFY column_name data_type

   For details, see :ref:`ALTER TABLE <dws_06_0142>`.

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notification instead of an error if no tables have identical names. The notification prompts that the table you are querying does not exist.

-  **tablename**

   Specifies the name of an existing foreign table to be modified.

   Value range: an existing foreign table name

-  **new_owner**

   Specifies the new owner of the foreign table.

   Value range: a string indicating a valid user name

-  **data_type**

   Specifies the new type for an existing column.

   Value range: a string. It must comply with the naming convention.

-  **column_name**

   Specifies the name of an existing column.

   Value range: a string. It must comply with the naming convention.

For details on how to modify other parameters in the foreign table, see :ref:`Parameter Description <en-us_topic_0000001510281989__s3e87132692794964b56e3ba420e7b544>` in **ALTER TABLE**.

Example:
--------

Modify the **region** option of the foreign table.

::

   ALTER FOREIGN TABLE region OPTIONS (SET schema_name 'test');

Change the type of the **r_name** column to **text** in the **region** foreign table.

::

   ALTER FOREIGN TABLE region ALTER r_name TYPE TEXT;

Helpful Links
-------------

:ref:`DROP FOREIGN TABLE <dws_06_0192>`, :ref:`CREATE FOREIGN TABLE (SQL on other GaussDB(DWS)) <dws_06_0162>`
