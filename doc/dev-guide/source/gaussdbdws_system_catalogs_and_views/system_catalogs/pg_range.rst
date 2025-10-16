:original_name: dws_04_0609.html

.. _dws_04_0609:

PG_RANGE
========

**PG_RANGE** records information about range types.

This is in addition to the types' entries in :ref:`PG_TYPE <dws_04_0629>`.

.. table:: **Table 1** PG_RANGE columns

   +--------------+---------+---------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | Column       | Type    | Reference                             | Description                                                                                                   |
   +==============+=========+=======================================+===============================================================================================================+
   | rngtypid     | OID     | :ref:`PG_TYPE <dws_04_0629>`.oid      | OID of the range type                                                                                         |
   +--------------+---------+---------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | rngsubtype   | OID     | :ref:`PG_TYPE <dws_04_0629>`.oid      | OID of the element type (subtype) of this range type                                                          |
   +--------------+---------+---------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | rngcollation | OID     | :ref:`PG_COLLATION <dws_04_0579>`.oid | OID of the collation used for range comparisons, or 0 if none                                                 |
   +--------------+---------+---------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | rngsubopc    | OID     | :ref:`PG_OPCLASS <dws_04_0603>`.oid   | OID of the subtype's operator class used for range comparisons                                                |
   +--------------+---------+---------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | rngcanonical | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the function to convert a range value into canonical form, or 0 if none                                |
   +--------------+---------+---------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | rngsubdiff   | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the function to return the difference between two element values as **double precision**, or 0 if none |
   +--------------+---------+---------------------------------------+---------------------------------------------------------------------------------------------------------------+

**rngsubopc** (plus **rngcollation**, if the element type is collatable) determines the sort ordering used by the range type. **rngcanonical** is used when the element type is discrete.
