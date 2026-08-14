:original_name: dws_06_0361.html

.. _dws_06_0361:

REFRESH MATERIALIZED VIEW
=========================

Function
--------

REFRESH MATERIALIZED VIEW refreshes materialized views. This syntax is supported only by clusters of version 8.2.1.220 or later.

The refresh mode is specified by the REFRESH parameter in the :ref:`CREATE MATERIALIZED VIEW <dws_06_0357>` syntax. Currently, full refresh and scheduled refresh are supported.

Precautions
-----------

The refresh operation blocks the DML operations on the base table.

Syntax
------

::

   REFRESH MATERIALIZED VIEW
   {[schema.]materialized_view_name} [ [DEFAULT] | [ RESTRICT ] | [ CASCADE FORWARD ] | [ CASCADE BACKWARD ] | [ CASCADE ALL ] ]

Parameter Description
---------------------

.. table:: **Table 1** REFRESH MATERIALIZED VIEW parameters

   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+
   | Parameter              | Description                                                                                                                                                                                                                                                                                                                                               | Value Range |
   +========================+===========================================================================================================================================================================================================================================================================================================================================================+=============+
   | materialized_view_name | Specifies the name of the materialized view to be refreshed.                                                                                                                                                                                                                                                                                              | ``-``       |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+
   | DEFAULT                | Specifies default refresh. It means that a single materialized view is not refreshed only until the base table has changed.                                                                                                                                                                                                                               | ``-``       |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+
   | RESTRICT               | Specifies forced refresh. It means that a materialized view is refreshed regardless of whether the base table has changed.                                                                                                                                                                                                                                | ``-``       |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+
   | CASCADE FORWARD        | Indicates a cascading refresh in the upward direction, starts by refreshing the current materialized view, and then refreshes the nested materialized views that depend on it. The default refresh method is used.                                                                                                                                        | ``-``       |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+
   | CASCADE BACKWARD       | Specifies cascading refresh in the downward direction. The system refreshes the lowest-level materialized view that the current materialized view relies on, then continues to refresh the nested materialized views that depend on the lowest-level one, and finally refreshes the current materialized view itself. The default refresh method is used. | ``-``       |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+
   | CASCADE ALL            | Specifies all cascading refreshes. First, cascading refreshes are performed downward, and then cascading refreshes are performed upward.                                                                                                                                                                                                                  | ``-``       |
   +------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+

Examples
--------

Refresh a materialized view.

::

   SET enable_matview = ON;
   DROP TABLE IF EXISTS t;
   CREATE TABLE t (a int, b int);
   DROP MATERIALIZED VIEW IF EXISTS mv1;
   CREATE MATERIALIZED VIEW mv1 BUILD DEFERRED REFRESH ON DEMAND AS SELECT * FROM t;
   REFRESH MATERIALIZED VIEW mv1;

Perform a cascading refresh in the downward direction for materialized views.

::

   DROP TABLE IF EXISTS t1;
   DROP MATERIALIZED VIEW IF EXISTS mv1;
   DROP MATERIALIZED VIEW IF EXISTS mv2;
   CREATE TABLE t1(a int,b int);
   CREATE MATERIALIZED VIEW mv1 AS SELECT * FROM t1 where a > 0;
   CREATE MATERIALIZED VIEW mv2 AS SELECT * FROM mv1;
   INSERT INTO t1 values(1,1),(2,2);
   REFRESH MATERIALIZED VIEW mv2 cascade backward; -- After refreshing mv2, the data in mv1 was also updated.

   SELECT * FROM mv1 ORDER BY 1,2;

|image1|

::

   SELECT * FROM mv2 ORDER BY 1,2;

|image2|

Helpful Links
-------------

:ref:`CREATE MATERIALIZED VIEW <dws_06_0357>`, :ref:`ALTER MATERIALIZED VIEW <dws_06_0358>`, :ref:`DROP MATERIALIZED VIEW <dws_06_0360>`

.. |image1| image:: /_static/images/en-us_image_0000002617995731.png
.. |image2| image:: /_static/images/en-us_image_0000002587396514.png
