:original_name: dws_06_0305.html

.. _dws_06_0305:

Collation Version Function
==========================

pg_collation_actual_version (oid)
---------------------------------

Description: Returns the actual version of the collation object currently installed in the operating system. Currently, this parameter is valid only for case_insensitive collations.

Return type: text

Example:

::

   SELECT oid FROM pg_collation WHERE collname ='case_insensitive';
    oid
   ------
    3300
   (1 row)

   SELECT pg_collation_actual_version(3300);
    pg_collation_actual_version
   -----------------------------
    153.14
   (1 row)
