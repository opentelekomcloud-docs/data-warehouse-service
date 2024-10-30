:original_name: dws_06_0289.html

.. _dws_06_0289:

DROP SUBSCRIPTION
=================

Function
--------

Deletes a subscription.

Precautions
-----------

-  This statement is supported by clusters of version 8.2.0.100 or later.
-  A subscription can be deleted by the system administrator only.

Syntax
------

::

   DROP SUBSCRIPTION [ IF EXISTS ] name

Parameter Description
---------------------

-  **IF EXISTS**

   If the specified subscription does not exist, no error is thrown. Instead, a notification is reported indicating that the subscription does not exist.

-  **name**

   Specifies the name of the subscription you want to delete.

   Value range: an existing subscription

Examples
--------

Delete a subscription.

.. code-block::

   DROP SUBSCRIPTION mysub;

Helpful Links
-------------

:ref:`ALTER SUBSCRIPTION <dws_06_0287>`, :ref:`CREATE SUBSCRIPTION <dws_06_0288>`
