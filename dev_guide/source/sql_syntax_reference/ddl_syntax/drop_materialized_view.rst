:original_name: dws_06_0360.html

.. _dws_06_0360:

DROP MATERIALIZED VIEW
======================

Function
--------

Deletes a materialized view. This syntax is supported only by clusters of 8.2.1.100 or later.

Precautions
-----------

None

Syntax
------

::

   DROP MATERIALIZED VIEW [ IF EXISTS ]
   {[schema.]materialized_view_name} [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified materialized view does not exist.

-  **materialized_view_name**

   Name of the materialized view to be deleted.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: automatically deletes all objects that are contained in the schema to be deleted.
   -  **RESTRICT**: refuses to delete the schema that contains any objects. This is the default.

Examples
--------

Delete a materialized view.

::

   DROP MATERIALIZED VIEW mv1;

Helpful Links
-------------

:ref:`ALTER MATERIALIZED VIEW <dws_06_0358>`, :ref:`CREATE MATERIALIZED VIEW <dws_06_0357>`, :ref:`REFRESH MATERIALIZED VIEW <dws_06_0361>`
