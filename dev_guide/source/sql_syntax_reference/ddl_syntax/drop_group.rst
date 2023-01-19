:original_name: dws_06_0194.html

.. _dws_06_0194:

DROP GROUP
==========

Function
--------

**DROP GROUP** deletes a user group.

**DROP GROUP** is the alias for **DROP ROLE**.

Precautions
-----------

**DROP GROUP** is the internal interface encapsulated in the **gs_om** tool. You are not advised to use this interface, because doing so affects the cluster.

Syntax
------

::

   DROP GROUP [ IF EXISTS ] group_name [, ...];

Parameter Description
---------------------

See :ref:`Examples <en-us_topic_0000001145510617__s3ff92f5089cb4291b96221b5f2b85e18>` in **DROP ROLE**.

Helpful Links
-------------

:ref:`CREATE GROUP <dws_06_0164>`, :ref:`ALTER GROUP <dws_06_0127>`, :ref:`DROP ROLE <dws_06_0203>`
