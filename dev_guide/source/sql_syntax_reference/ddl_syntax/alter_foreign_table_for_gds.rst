:original_name: dws_06_0123.html

.. _dws_06_0123:

ALTER FOREIGN TABLE (for GDS)
=============================

Function
--------

**ALTER FOREIGN TABLE** modifies a foreign table.

Precautions
-----------

None

Syntax
------

-  Set the attributes of a foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ]  table_name
          OPTIONS ( {[ ADD | SET | DROP ] option ['value']}[, ... ]);

-  Set a new owner.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          OWNER TO new_owner;

Parameter Description
---------------------

-  **table_name**

   Specifies the name of an existing foreign table to be modified.

   Value range: an existing foreign table name.

-  **option**

   Name of the option to be modified.

   Value range: See :ref:`Parameter Description <en-us_topic_0000001098990722__s949bbfb7d67e4891ac3744b6ecf3bb2a>` in **CREATE FOREIGN TABLE**.

-  **value**

   Specifies the new value of **option**.

Examples
--------

Modify the **customer_ft** attribute of the foreign table. Delete the **mode** option.

::

   ALTER FOREIGN TABLE customer_ft options(drop mode);

Helpful Links
-------------

:ref:`CREATE FOREIGN TABLE (for GDS Import and Export) <dws_06_0159>`, :ref:`DROP FOREIGN TABLE <dws_06_0192>`
