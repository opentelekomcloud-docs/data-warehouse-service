:original_name: dws_06_0361.html

.. _dws_06_0361:

REFRESH MATERIALIZED VIEW
=========================

Function
--------

Refreshes a materialized view. The refresh mode is specified by the :ref:`REFRESH <en-us_topic_0000001811634773__li93006216815>` parameter in the **CREATE MATERIALIZED VIEW** syntax. Currently, full refresh and scheduled refresh are supported.

Precautions
-----------

The refresh operation blocks the DML operations on the base table.

Syntax
------

::

   REFRESH MATERIALIZED VIEW
   {[schema.]materialized_view_name}

Parameter Description
---------------------

-  **materialized_view_name**

   Indicates the name of the materialized view to be refreshed.

Examples
--------

Refresh a materialized view.

::

   REFRESH MATERIALIZED VIEW mv1;

Helpful Links
-------------

:ref:`CREATE MATERIALIZED VIEW <dws_06_0357>`, :ref:`ALTER MATERIALIZED VIEW <dws_06_0358>`, :ref:`DROP MATERIALIZED VIEW <dws_06_0360>`
