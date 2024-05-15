:original_name: dws_06_0215.html

.. _dws_06_0215:

DROP VIEW
=========

Function
--------

**DROP VIEW** forcibly deletes an existing view in a database.

Precautions
-----------

-  Only a view owner or a system administrator can run **DROP VIEW** command.
-  The database stores only the definition of a view, but does not store the data corresponding to the view. If the base table remains unchanged, you can run the :ref:`CREATE VIEW <dws_06_0187>` command to recreate a view that is deleted by mistake.

Syntax
------

::

   DROP VIEW [ IF EXISTS ] view_name [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of an error if the specified view does not exist.

-  **view_name**

   Specifies the name of the view to be deleted.

   Value range: An existing view.

-  **CASCADE \| RESTRICT**

   -  **CASCADE**: deletes objects (such as other views) that depend on a view to be deleted.
   -  **RESTRICT**: refuses to delete the view if any objects depend on it. This is the default.

Examples
--------

Delete the **myView** view:

::

   DROP VIEW myView;

Delete the **customer_details_view_v2** view:

::

   DROP VIEW public.customer_details_view_v2;

Helpful Links
-------------

:ref:`ALTER VIEW <dws_06_0150>`, :ref:`CREATE VIEW <dws_06_0187>`
