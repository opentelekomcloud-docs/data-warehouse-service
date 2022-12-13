:original_name: dws_03_0079.html

.. _dws_03_0079:

How Can I Export All Tables and Views from a Database?
======================================================

You can use **pg_tables** and **pg_views** to query all table information and views in a database. Example:

.. code-block::

   SELECT * FROM pg_tables;
   SELECT * FROM pg_views;

For details about the returned columns, see "PG_TABLES" and "PG_VIEWS" in the *Developer Guide*.
