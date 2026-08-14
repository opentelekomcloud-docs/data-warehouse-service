:original_name: dws_06_0215.html

.. _dws_06_0215:

DROP VIEW
=========

Function
--------

**DROP VIEW** deletes an existing view.

Precautions
-----------

Only a view owner or a system administrator can run **DROP VIEW** command.

|image1|

-  Be cautious when using **DROP OBJECT** (e.g., **DATABASE**, **USER/ROLE**, **SCHEMA**, **TABLE**, **VIEW**) as it may cause data loss, especially with **CASCADE** deletions. Always back up data before proceeding.
-  For more information about development and design specifications, see "DWS Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

::

   DROP VIEW [ IF EXISTS ] view_name [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

.. table:: **Table 1** DROP VIEW parameters

   +-----------------------+-----------------------------------------------------------------------------------------------+-----------------------+
   | Parameter             | Description                                                                                   | Value Range           |
   +=======================+===============================================================================================+=======================+
   | IF EXISTS             | Sends a notice instead of an error if the specified view does not exist.                      | ``-``                 |
   +-----------------------+-----------------------------------------------------------------------------------------------+-----------------------+
   | view_name             | Specifies the name of the view to be deleted.                                                 | An existing view.     |
   +-----------------------+-----------------------------------------------------------------------------------------------+-----------------------+
   | CASCADE \| RESTRICT   | Specifies the policy for processing dependent objects when an existing view is deleted.       | ``-``                 |
   |                       |                                                                                               |                       |
   |                       | -  **CASCADE**: deletes objects (such as other views) that depend on a view to be deleted.    |                       |
   |                       | -  **RESTRICT**: refuses to delete the view if any objects depend on it. This is the default. |                       |
   +-----------------------+-----------------------------------------------------------------------------------------------+-----------------------+

Examples
--------

Delete the view **myView**.

::

   DROP VIEW IF EXISTS myView;

Delete the view **customer_details_view_v2**.

::

   DROP VIEW IF EXISTS public.customer_details_view_v2;

Helpful Links
-------------

:ref:`ALTER VIEW <dws_06_0150>`, :ref:`CREATE VIEW <dws_06_0187>`

.. |image1| image:: /_static/images/danger_3.0-en-us.png
