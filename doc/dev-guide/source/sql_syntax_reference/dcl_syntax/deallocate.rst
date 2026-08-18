:original_name: dws_06_0246.html

.. _dws_06_0246:

DEALLOCATE
==========

Function
--------

Deallocates a previously prepared statement. If a prepared statement is not explicitly deleted, it is deleted at the end of the session.

For details about prepared statements, see :ref:`PREPARE <dws_06_0251>`.

Precautions
-----------

None

Syntax
------

::

   DEALLOCATE [ PREPARE ] { name | ALL };

Parameter Description
---------------------

.. table:: **Table 1** DEALLOCATE parameters

   +-----------+----------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Parameter | Description                                                                            | Value Range                                                               |
   +===========+========================================================================================+===========================================================================+
   | PREPARE   | Creates a prepared statement. This keyword is optional and does not affect operations. | ``-``                                                                     |
   +-----------+----------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | name      | Specifies the name of the prepared statement to be deleted.                            | A string, which must comply with the naming rules of prepared statements. |
   +-----------+----------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | ALL       | Deallocates all prepared statements.                                                   | A string, which must comply with the prepared statement format.           |
   +-----------+----------------------------------------------------------------------------------------+---------------------------------------------------------------------------+

Examples
--------

None
