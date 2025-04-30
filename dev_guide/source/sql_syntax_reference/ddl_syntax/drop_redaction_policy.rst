:original_name: dws_06_0199.html

.. _dws_06_0199:

DROP REDACTION POLICY
=====================

Function
--------

**DROP REDACTION POLICY** deletes a data masking policy applied to a specified table.

Precautions
-----------

Only the table owner and users granted the **gs_redaction_policy** preset role have the permission to delete data masking policies.

Syntax
------

::

   DROP REDACTION POLICY [ IF EXISTS ] policy_name ON table_name;

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notice instead of throwing an error if the redaction policy to be deleted does not exist.

-  **policy_name**

   Specifies the name of the masking policy to be deleted.

-  **table_name**

   Specifies the name of the table with the masking policy to be deleted.

Examples
--------

Delete the masking policy **mask_emp** from table **emp**:

::

   DROP REDACTION POLICY mask_emp ON emp;

Helpful Links
-------------

:ref:`ALTER REDACTION POLICY <dws_06_0132>`, :ref:`CREATE REDACTION POLICY <dws_06_0168>`
