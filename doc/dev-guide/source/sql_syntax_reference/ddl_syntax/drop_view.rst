:original_name: dws_06_0215.html

.. _dws_06_0215:

DROP VIEW
=========

Function
--------

**DROP VIEW** forcibly deletes an existing view in a database.

Precautions
-----------

Only a view owner or a system administrator can run **DROP VIEW** command.

|image1|

-  Be cautious when using **DROP OBJECT** (e.g., **DATABASE**, **USER/ROLE**, **SCHEMA**, **TABLE**, **VIEW**) as it may cause data loss, especially with **CASCADE** deletions. Always back up data before proceeding.
-  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

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

.. |image1| image:: /_static/images/danger_3.0-en-us.png
