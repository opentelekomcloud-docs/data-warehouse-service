:original_name: dws_06_0150.html

.. _dws_06_0150:

ALTER VIEW
==========

Function
--------

**ALTER VIEW** modifies all auxiliary attributes of a view. (To modify the query definition of a view, use **CREATE OR REPLACE VIEW**.)

Precautions
-----------

-  Only the view owner can modify a view by running **ALTER VIEW**.
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

-  **IF EXISTS**

   If this option is specified, no error is reported if the view does not exist. Only a message is displayed.

-  **view_name**

   Specifies the view name, which can be schema-qualified.

   Value range: a string. It must comply with the naming convention.

-  **column_name**

   Indicates an optional list of names to be used for columns of the view. If not given, the column names are deduced from the query.

   Value range: a string. It must comply with the naming convention.

-  **SET/DROP DEFAULT**

   Sets or deletes the default value of a column. Currently, this parameter does not take effect.

-  **new_owner**

   Specifies the new owner of a view.

-  **new_name**

   Specifies the new view name.

-  **new_schema**

   Specifies the new schema of the view.

-  **view_option_name [ = view_option_value ]**

   This clause specifies optional parameters for a view.

   Currently, the only parameter supported by **view_option_name** is **security_barrier**, which should be enabled when a view is intended to provide row-level security.

   Value range: boolean type. It can be **TRUE** or **FALSE**.

-  **REBUILD**

   This clause is used for view decoupling. You can use the saved original statement to rebuild views and restore the dependencies. In 9.1.0.200 and later cluster versions, the view decoupling function does not require **view_independent** to be enabled. If a view becomes invalid, it can simply be rebuilt.

   Note the following:

   -  View rebuilding starts from the current view and updates all associated backward views. If the forward views on which the current view depends are also unavailable, automatic rebuilding is triggered.
   -  The temporary tables and views that have dependency relationships cannot be decoupled and dropped. However, you can perform the REBUILD operation on temporary views that do not have dependency relationships.
   -  View schema names and view names can be modified. The names of rebuilt view schemas or views are re-created based on the latest name, but the query operation retains the original definition.
   -  Only fields of the character, number, and time types in the base table can be modified.Only fields of the character, number, and time types in the base table can be modified. When a field is added to the base table, the view is not invalidated and the definition remains unchanged.
   -  Invalid views are exported as comments during backup. You need to manually restore the invalid views.

   .. note::

      The upper-layer cascading views become invalid in the following scenarios:

      -  DROP TABLE/VIEW
      -  RENAME TABLE/VIEW
      -  ALTER TABLE DROP COLUMN
      -  ALTER TABLE CHANGE/ALTER COLUMN TYPE
      -  ALTER TABLE CHANGE/ALTER COLUMN NAME
      -  ALTER TABLE/VIEW NAMESPACE
      -  ALTER TABLE/VIEW RENAME

-  **ONLY**

   Only views and their dependent views are rebuilt. This function does not require **view_independent** to be enabled. If a view becomes invalid, it can simply be rebuilt.

Examples
--------

Rename a view.

::

   ALTER VIEW tpcds.customer_details_view_v1 RENAME TO customer_details_view_v2;

Change a view name and prefix the new view name with the schema name of the original view.

::

   ALTER VIEW tpcds.customer_details_view_v1 RENAME TO tpcds.customer_details_view_v2;

Change the schema of a view.

::

   ALTER VIEW tpcds.customer_details_view_v2 SET schema public;

Rebuild a view.

::

   ALTER VIEW public.customer_details_view_v2 REBUILD;

Rebuild a dependent view.

::

   ALTER VIEW ONLY public.customer_details_view_v2 REBUILD;

Helpful Links
-------------

:ref:`CREATE VIEW <dws_06_0187>`, :ref:`DROP VIEW <dws_06_0215>`
