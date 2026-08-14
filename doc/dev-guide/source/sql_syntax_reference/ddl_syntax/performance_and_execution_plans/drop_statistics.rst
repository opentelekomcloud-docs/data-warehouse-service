:original_name: dws_06_0365.html

.. _dws_06_0365:

DROP STATISTICS
===============

Function
--------

Deletes an extended statistics object. This syntax is supported only by clusters of version 8.2.1.200 or later (not supported by clusters of version 8.3.0).

Precautions
-----------

None

Syntax
------

::

   DROP STATISTICS [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]

Parameter Description
---------------------

.. table:: **Table 1** DROP STATISTICS parameters

   +---------------------+-----------------------------------------------------------------------------------------------------------------+----------------------------------------------+
   | Parameter           | Description                                                                                                     | Value Range                                  |
   +=====================+=================================================================================================================+==============================================+
   | IF EXISTS           | Sends a notification rather than reporting an error if the specified extended statistics object does not exist. | ``-``                                        |
   +---------------------+-----------------------------------------------------------------------------------------------------------------+----------------------------------------------+
   | name                | Indicates the name of the extended statistics object to be deleted.                                             | An existing extended statistics object name. |
   +---------------------+-----------------------------------------------------------------------------------------------------------------+----------------------------------------------+
   | CASCADE \| RESTRICT | This parameter does not take effect. No objects depend on extended statistics objects.                          | ``-``                                        |
   +---------------------+-----------------------------------------------------------------------------------------------------------------+----------------------------------------------+

Examples
--------

Deletes an extended statistics object.

.. code-block::

   DROP STATISTICS s1_t1_row;

Helpful Links
-------------

:ref:`CREATE STATISTICS <dws_06_0364>`
