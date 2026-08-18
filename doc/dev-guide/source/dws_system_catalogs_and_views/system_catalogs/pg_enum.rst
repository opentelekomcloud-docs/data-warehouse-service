:original_name: dws_04_0588.html

.. _dws_04_0588:

PG_ENUM
=======

**PG_ENUM** records entries showing the values and labels for each enum type. The internal representation of a given enum value is actually the OID of its associated row in **pg_enum**.

.. table:: **Table 1** PG_ENUM columns

   +---------------+------+----------------------------------+----------------------------------------------------------------------------+
   | Column        | Type | Reference                        | Description                                                                |
   +===============+======+==================================+============================================================================+
   | OID           | OID  | N/A                              | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +---------------+------+----------------------------------+----------------------------------------------------------------------------+
   | enumtypid     | OID  | :ref:`PG_TYPE <dws_04_0629>`.oid | OID of **pg_type** that contains this enum value                           |
   +---------------+------+----------------------------------+----------------------------------------------------------------------------+
   | enumsortorder | Real | N/A                              | Sort position of this enum value within its enum type                      |
   +---------------+------+----------------------------------+----------------------------------------------------------------------------+
   | enumlabel     | Name | N/A                              | Textual label for this enum value                                          |
   +---------------+------+----------------------------------+----------------------------------------------------------------------------+

The OIDs for **PG_ENUM** rows follow a special rule: even-numbered OIDs are guaranteed to be ordered in the same way as the sort ordering of their enum type. That is, if two even OIDs belong to the same enum type, the smaller OID must have the smaller **enumsortorder** value. Odd-numbered OID values need bear no relationship to the sort order. This rule allows the enum comparison routines to avoid catalog lookups in many common cases. The routines that create and alter enum types attempt to assign even OIDs to enum values whenever possible.

When an enum type is created, its members are assigned sort-order positions from 1 to *n*. But members added later might be given negative or fractional values of **enumsortorder**. The only requirement on these values is that they be correctly ordered and unique within each enum type.
