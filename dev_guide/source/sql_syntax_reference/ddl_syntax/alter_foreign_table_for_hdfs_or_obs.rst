:original_name: dws_06_0124.html

.. _dws_06_0124:

ALTER FOREIGN TABLE (for HDFS or OBS)
=====================================

Function
--------

Modifies an HDFS or OBS foreign table.

Precautions
-----------

None

Syntax
------

-  Set a foreign table's attributes:

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ]  table_name
          OPTIONS ( {[ ADD | SET | DROP ] option ['value']} [, ... ]);

-  Set the owner of the foreign table:

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          OWNER TO new_owner;

-  Update a foreign table column:

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] table_name
          MODIFY ( { column_name data_type | column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] | column_name [ CONSTRAINT constraint_name ] NULL } [, ...] );

-  Modify the column of the foreign table:

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          action [, ... ];

   The **action** syntax is as follows:

   ::

      ALTER [ COLUMN ] column_name [ SET DATA ] TYPE data_type
         | ALTER [ COLUMN ] column_name { SET | DROP } NOT NULL
         | ALTER [ COLUMN ] column_name SET STATISTICS [PERCENT] integer
         | ALTER [ COLUMN ] column_name OPTIONS ( {[ ADD | SET | DROP ] option ['value'] } [, ... ])
         | MODIFY column_name data_type
         | MODIFY column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ]
         | MODIFY column_name [ CONSTRAINT constraint_name ] NULL

   For details, see :ref:`ALTER TABLE <dws_06_0142>`.

-  Add a foreign table informational constraint:

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          ADD [ CONSTRAINT constraint_name ]
          { PRIMARY KEY | UNIQUE } ( column_name )
          [ NOT ENFORCED [ ENABLE QUERY OPTIMIZATION | DISABLE QUERY OPTIMIZATION ] | ENFORCED ];

   For parameters about adding an informational constraint to a foreign table, see :ref:`Parameter Description <en-us_topic_0000001811634745__s755e54aa01f04a4bb44806bedcebdab4>`.

-  Remove a foreign table informational constraint.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          DROP CONSTRAINT constraint_name ;

-  Set the row-level access control switch (supported by cluster versions 9.1.0 and later).

   .. code-block::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
              ENABLE | DISABLE | FORCE | NO FORCE ROW LEVEL SECURITY

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notification instead of an error if no tables have identical names. The notification prompts that the table you are querying does not exist.

-  **tablename**

   Specifies the name of an existing foreign table to be modified.

   Value range: an existing foreign table name

-  **new_owner**

   Specifies the new owner of the foreign table.

   Value range: A string indicating a valid user name.

-  **data_type**

   Specifies the new type for an existing column.

   Value range: A string compliant with the identifier naming rules.

-  **constraint_name**

   Specifies the name of a constraint to add or delete.

-  **column_name**

   Specifies the name of an existing column.

   Value range: a string. It must comply with the naming convention.

For details on how to modify other parameters in the foreign table, such as **IF EXISTS**, see :ref:`Parameter Description <en-us_topic_0000001811634545__s3e87132692794964b56e3ba420e7b544>` in **ALTER TABLE**.

.. _en-us_topic_0000001764516270__s8302a739997543e0a22f9ee43ce9bfbf:

Examples
--------

Change the type of the **r_name** column to **text** in the **ft_region** foreign table.

::

   ALTER FOREIGN TABLE ft_region ALTER r_name TYPE TEXT;

Run the following command to mark the **r_name** column of the **ft_region** foreign table as **not null**:

::

   ALTER FOREIGN TABLE ft_region ALTER r_name SET NOT NULL;

Helpful Links
-------------

:ref:`CREATE FOREIGN TABLE (SQL on OBS or Hadoop) <dws_06_0161>`, :ref:`DROP FOREIGN TABLE <dws_06_0192>`
