:original_name: dws_06_0150.html

.. _dws_06_0150:

ALTER VIEW
==========

Function
--------

**ALTER VIEW** modifies all auxiliary attributes of a view. (To modify the query definition of a view, use **CREATE OR REPLACE VIEW**.)

Precautions
-----------

-  Only the view owner can run **ALTER VIEW**.
-  To change a view's schema, you must also have the CREATE permission on the new schema.
-  To alter the owner, you must also be a direct or indirect member of the new owning role, and that role must have CREATE privilege on the view's schema.
-  An administrator can change the owner relationship of any view.

Syntax
------

-  Set the default value of the view column.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          ALTER [ COLUMN ] column_name SET DEFAULT expression;

-  Remove the default value of the view column.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          ALTER [ COLUMN ] column_name DROP DEFAULT;

-  Change the owner of a view.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          OWNER TO new_owner;

-  Renames a view. The new view name can be prefixed with the schema name of the original view. The schema name cannot be changed at the same time.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          RENAME TO new_name;
      ALTER VIEW [ IF EXISTS ] view_name
          RENAME TO schema.new_name;

-  Set the schema of the view.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          SET SCHEMA new_schema;

-  Set the options of the view.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          SET ( { view_option_name [ = view_option_value ] } [, ... ] );

-  Reset the options of the view.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          RESET ( view_option_name [, ... ] );

-  Rebuild the current view and its lower-layer and upper-layer dependent views.

   ::

      ALTER VIEW [ IF EXISTS ] view_name
          REBUILD;

-  Rebuild the current view and its lower-layer dependent views.

   ::

      ALTER VIEW [ IF EXISTS ] ONLY view_name
          REBUILD;

Parameter Description
---------------------

.. table:: **Table 1** ALTER VIEW parameters

   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                                     | Description                                                                                                                                                                                                                                                                                                      | Value Range                                                                                                                             |
   +===============================================================+==================================================================================================================================================================================================================================================================================================================+=========================================================================================================================================+
   | IF EXISTS                                                     | Sends a prompt instead of an error if the view does not exist.                                                                                                                                                                                                                                                   | ``-``                                                                                                                                   |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | view_name                                                     | Specifies the name of the view to be modified, which can include a schema.                                                                                                                                                                                                                                       | Valid view name.                                                                                                                        |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | column_name                                                   | Indicates an optional list of names to be used for columns of the view. If not given, the column names are deduced from the query.                                                                                                                                                                               | Valid column name.                                                                                                                      |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | SET/DROP DEFAULT                                              | Sets or deletes the default value of a column. Currently, this parameter does not take effect.                                                                                                                                                                                                                   | ``-``                                                                                                                                   |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | new_owner                                                     | Specifies the new owner of the view.                                                                                                                                                                                                                                                                             | Valid username.                                                                                                                         |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | new_name                                                      | Specifies the new view name.                                                                                                                                                                                                                                                                                     | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                        |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | new_schema                                                    | Specifies the new schema of the view.                                                                                                                                                                                                                                                                            | Valid schema name.                                                                                                                      |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | SET ( { view_option_name [ = view_option_value ] } [, ... ] ) | This clause specifies optional parameters for a view.                                                                                                                                                                                                                                                            | **security_barrier** is of the boolean type. The options include:                                                                       |
   |                                                               |                                                                                                                                                                                                                                                                                                                  |                                                                                                                                         |
   |                                                               | Currently, **view_option_name** can only be set to **security_barrier**.                                                                                                                                                                                                                                         | -  TRUE                                                                                                                                 |
   |                                                               |                                                                                                                                                                                                                                                                                                                  | -  FALSE                                                                                                                                |
   |                                                               | You can use **WITH (security_barriers)** to create a relatively secure view. This prevents attackers from printing hidden base table data by using the **RAISE** statement of low-cost functions.                                                                                                                |                                                                                                                                         |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | REBUILD                                                       | This clause is used for view decoupling. You can use the saved original statement to rebuild views and restore the dependencies. In clusters of 9.1.0.200 or later, the GUC parameter **view_independent** does not need to be enabled for view decoupling. If a view is detected as invalid, it can be rebuilt. | For how to use **REBUILD**, see :ref:`REBUILD (View Decoupling) Usage Description <en-us_topic_0000001811515609__section180715581255>`. |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | ONLY                                                          | Limits view rebuilding to only the selected view and its dependencies. This function does not require the GUC parameter **view_independent** to be enabled. If a view is detected as invalid, it can be rebuilt.                                                                                                 | ``-``                                                                                                                                   |
   +---------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001811515609__section180715581255:

REBUILD (View Decoupling) Usage Description
-------------------------------------------

-  View rebuilding starts from the current view and updates all associated backward views. If the forward views on which the current view depends are also unavailable, automatic rebuilding is triggered.
-  The temporary tables and views that have dependency relationships cannot be decoupled and dropped. However, you can perform the REBUILD operation on temporary views that do not have dependency relationships.
-  View schema names and view names can be modified. The names of rebuilt view schemas or views are re-created based on the latest name, but the query operation retains the original definition.
-  Only fields of the character, number, and time types in the base table can be modified.Only fields of the character, number, and time types in the base table can be modified. When a field is added to the base table, the view is not invalidated and the definition remains unchanged.
-  Invalid views are exported as comments during backup. You need to manually restore the invalid views.
-  The upper-layer cascading views are invalid in the following scenarios:

   -  DROP TABLE/VIEW
   -  RENAME TABLE/VIEW
   -  ALTER TABLE DROP COLUMN
   -  ALTER TABLE CHANGE/ALTER COLUMN TYPE
   -  ALTER TABLE CHANGE/ALTER COLUMN NAME
   -  ALTER TABLE/VIEW NAMESPACE
   -  ALTER TABLE/VIEW RENAME

Examples
--------

Create the view **myview**.

::

   CREATE OR REPLACE VIEW myview AS
       SELECT * FROM pg_tablespace WHERE spcname = 'pg_default';

Change the view name to **product_view**.

::

   ALTER VIEW myview RENAME TO product_view;

Change the schema of the **product_view** view to **schetest**.

::

   CREATE SCHEMA schetest;
   ALTER VIEW product_view SET schema schetest;

Rebuild the view **schetest.product_view**.

::

   ALTER VIEW schetest.product_view REBUILD;

Rebuild the dependent view **schetest.product_view**.

::

   ALTER VIEW ONLY schetest.product_view REBUILD;

Helpful Links
-------------

:ref:`CREATE VIEW <dws_06_0187>` and :ref:`DROP VIEW <dws_06_0215>`
