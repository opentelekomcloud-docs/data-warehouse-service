:original_name: dws_06_0148.html

.. _dws_06_0148:

ALTER TYPE
==========

Function
--------

**ALTER TYPE** modifies the definition of a type.

Precautions
-----------

-  Only type owners can run **ALTER TYPE**.
-  To modify the schema of a type, you must also have the **CREATE** permission for the new schema.
-  To change the owner, you must belong directly or indirectly to the new owner's role. You also need the **CREATE** permission on the schema of the type.
-  System administrators can change the ownership of any type as needed.
-  To add an attribute or modify the type of an attribute, you must have the **USAGE** permission on the attribute type.

Syntax
------

-  Modify a type.

   ::

      ALTER TYPE name action [, ... ];
      ALTER TYPE name OWNER TO { new_owner | CURRENT_USER | SESSION_USER };
      ALTER TYPE name RENAME ATTRIBUTE attribute_name TO new_attribute_name [ CASCADE | RESTRICT ];
      ALTER TYPE name RENAME TO new_name;
      ALTER TYPE name SET SCHEMA new_schema;
      ALTER TYPE name ADD VALUE [ IF NOT EXISTS ] new_enum_value [ { BEFORE | AFTER } neighbor_enum_value ];
      ALTER TYPE name RENAME VALUE existing_enum_value TO new_enum_value;

      where action is one of:
          ADD ATTRIBUTE attribute_name data_type [ COLLATE collation ] [ CASCADE | RESTRICT ];
          DROP ATTRIBUTE [ IF EXISTS ] attribute_name [ CASCADE | RESTRICT ];
          ALTER ATTRIBUTE attribute_name [ SET DATA ] TYPE data_type [ COLLATE collation ] [ CASCADE | RESTRICT ];

-  Add a new attribute to a composite type.

   ::

      ALTER TYPE name ADD ATTRIBUTE attribute_name data_type [ COLLATE collation ] [ CASCADE | RESTRICT ];

-  Delete an attribute from a composite type.

   ::

      ALTER TYPE name DROP ATTRIBUTE [ IF EXISTS ] attribute_name [ CASCADE | RESTRICT ];

-  Change the type of an attribute in a composite type.

   ::

      ALTER TYPE name ALTER ATTRIBUTE attribute_name [ SET DATA ] TYPE data_type [ COLLATE collation ] [ CASCADE | RESTRICT ];

.. note::

   **ADD ATTRIBUTE**, **DROP ATTRIBUTE**, and **ALTER ATTRIBUTE** can be combined for execution. For example, you can add multiple attributes to a command or change the types of multiple attributes.

-  Change the owner of a type.

   ::

      ALTER TYPE name OWNER TO { new_owner | CURRENT_USER | SESSION_USER };

-  Change the name of a type or the name of an attribute in a composite type.

   ::

      ALTER TYPE name RENAME TO new_name;
      ALTER TYPE name RENAME ATTRIBUTE attribute_name TO new_attribute_name [ CASCADE | RESTRICT ];

-  Move a type to a new schema.

   ::

      ALTER TYPE name SET SCHEMA new_schema;

-  Add a new value to an enumerated type.

   ::

      ALTER TYPE name ADD VALUE [ IF NOT EXISTS ] new_enum_value [ { BEFORE | AFTER } neighbor_enum_value ];

-  Change an enumerated value in the value list.

   ::

      ALTER TYPE name RENAME VALUE existing_enum_value TO new_enum_value;

Parameter Description
---------------------

.. table:: **Table 1** ALTER TYPE parameters

   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | Parameter           | Description                                                                                                               | Value Range                                                             |
   +=====================+===========================================================================================================================+=========================================================================+
   | name                | Specifies the name of an existing type to be modified, which can include a schema.                                        | Valid type name.                                                        |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | new_name            | Specifies the new name of a type.                                                                                         | A string.                                                               |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | new_owner           | Specifies the username of the new owner of the type.                                                                      | Valid username.                                                         |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | new_schema          | Specifies the new schema of the type.                                                                                     | Valid schema name.                                                      |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | attribute_name      | Specifies the name of the attribute to be added, changed, or deleted.                                                     | ``-``                                                                   |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | new_attribute_name  | Specifies the new name of the property to be renamed.                                                                     | ``-``                                                                   |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | data_type           | Specifies the data type of the attribute to be added or the new type name of the attribute to be changed.                 | ``-``                                                                   |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | new_enum_value      | Specifies the new tag value of the enumeration type.                                                                      | A string. The value cannot be empty and contains a maximum of 64 bytes. |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | neighbor_enum_value | Specifies an existing enumerated value before or after which a new enumerated value will be added.                        | ``-``                                                                   |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | existing_enum_value | Specifies the existing enumeration value to be renamed.                                                                   | A string. The value cannot be empty and contains a maximum of 64 bytes. |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | CASCADE             | Determines that the type to be modified, its associated records, and subtables that inherit the type will all be updated. | ``-``                                                                   |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+
   | RESTRICT            | Refuses the update if the type of the association to be updated is an updated type. **RESTRICT** is the default option.   | ``-``                                                                   |
   +---------------------+---------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------+

Examples
--------

Create composite type **test**, enumeration type **testdata**, and user **user_t**.

::

   CREATE TYPE test AS (col1 int, col text);
   CREATE TYPE testdata AS ENUM ('create', 'modify', 'closed');
   CREATE USER user_t PASSWORD '{Password}';

Rename data type **test** to **test1**.

::

   ALTER TYPE test RENAME TO test1;

Change the owner of the user-defined type **test1** to **user_t**.

::

   ALTER TYPE test1 OWNER TO user_t;

Change the schema of the user-defined type **test1** to **user_t**.

::

   ALTER TYPE test1 SET SCHEMA user_t;

Add a new attribute **col3** to the user-defined type **test1**.

::

   ALTER TYPE user_t.test1 ADD ATTRIBUTE col3 int;

Add a tag value to the enumeration type **testdata**.

::

   ALTER TYPE testdata ADD VALUE IF NOT EXISTS 'regress' BEFORE 'closed';

Rename a tag value of the enumeration type **testdata**.

::

   ALTER TYPE testdata RENAME VALUE 'create' TO 'new';

Helpful Links
-------------

:ref:`CREATE TYPE <dws_06_0185>` and :ref:`DROP TYPE <dws_06_0213>`
