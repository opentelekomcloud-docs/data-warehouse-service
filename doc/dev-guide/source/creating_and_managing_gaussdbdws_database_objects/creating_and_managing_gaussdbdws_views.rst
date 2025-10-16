:original_name: dws_04_0039.html

.. _dws_04_0039:

Creating and Managing GaussDB(DWS) Views
========================================

Views allow users to save queries. Views are not physically stored on disks. Queries to a view run as subqueries. A database only stores the definition of a view and does not store its data. The data is still stored in the original base table. If data in the base table changes, the data in the view changes accordingly. In this sense, a view is like a window through which users can know their interested data and data changes in the database. A view is triggered every time it is referenced.

Creating a view
---------------

Run the **CREATE VIEW** command to create a view.

::

   CREATE OR REPLACE VIEW MyView AS SELECT * FROM tpcds.customer WHERE c_customer_sk < 150;

.. note::

   The **OR REPLACE** parameter in this command is optional. It indicates that if the view exists, the new view will replace the existing view.

View Details
------------

-  View the *MyView* view. Real-time data will be returned.

   ::

      SELECT * FROM myview;

-  Run the following command to query the views in the current user:

   ::

      SELECT * FROM user_views;

-  Run the following command to query all views:

   ::

      SELECT * FROM dba_views;

-  View details about a specified view.

   Run the following command to view details about the dba_users view:

   ::

      \d+ dba_users
                            View "PG_CATALOG.DBA_USERS"
        Column  |         Type          | Modifiers | Storage  | Description
      ----------+-----------------------+-----------+----------+-------------
       USERNAME | CHARACTER VARYING(64) |           | extended |
      View definition:
       SELECT PG_AUTHID.ROLNAME::CHARACTER VARYING(64) AS USERNAME
         FROM PG_AUTHID;

Rebuilding a View
-----------------

Run the **ALTER VIEW** command to rebuild a view without entering query statements.

::

   ALTER VIEW myview REBUILD;

Deleting a View
---------------

Run the **DROP VIEW** command to delete a view.

::

   DROP VIEW myview;

DROP VIEW ... The **CASCADE** command can be used to delete objects that depend on the view. For example, view A depends on view B. If view B is deleted, view A will also be deleted. Without the CASCADE option, the **DROP VIEW** command will fail.
