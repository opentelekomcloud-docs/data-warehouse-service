:original_name: dws_06_0199.html

.. _dws_06_0199:

DROP REDACTION POLICY
=====================

Function
--------

Deletes a data redaction/masking policy applied to a specified table.

Precautions
-----------

Only the table owner has the permission to delete a data redaction policy.

Syntax
------

::

   DROP REDACTION POLICY [ IF EXISTS ] policy_name ON table_name;

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of throwing an error if the redaction policy to be deleted does not exist.

-  **policy_name**

   Specifies the name of the redaction policy to be deleted.

-  **table_name**

   Specifies the name of the table to which the redaction policy is applied.

Examples
--------

Delete the redaction policy **mask_emp** from table **emp**:

::

   DROP REDACTION POLICY mask_emp ON emp;

Helpful Links
-------------

:ref:`ALTER REDACTION POLICY <dws_06_0132>`, :ref:`CREATE REDACTION POLICY <dws_06_0168>`
