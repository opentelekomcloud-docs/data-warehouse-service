:original_name: dws_04_0571.html

.. _dws_04_0571:

PG_AMPROC
=========

**PG_AMPROC** records information about the support procedures associated with the access method operator families. There is one row for each support procedure belonging to an operator family.

.. table:: **Table 1** PG_AMPROC columns

   +-----------------+----------+--------------------------------------+----------------------------------------------------------------------------+
   | Name            | Type     | Reference                            | Description                                                                |
   +=================+==========+======================================+============================================================================+
   | OID             | OID      | N/A                                  | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +-----------------+----------+--------------------------------------+----------------------------------------------------------------------------+
   | amprocfamily    | OID      | :ref:`PG_OPFAMILY <dws_04_0605>`.oid | Operator family this entry is for                                          |
   +-----------------+----------+--------------------------------------+----------------------------------------------------------------------------+
   | amproclefttype  | OID      | :ref:`PG_TYPE <dws_04_0629>`.oid     | Left-hand input data type of associated operator                           |
   +-----------------+----------+--------------------------------------+----------------------------------------------------------------------------+
   | amprocrighttype | OID      | :ref:`PG_TYPE <dws_04_0629>`.oid     | Right-hand input data type of associated operator                          |
   +-----------------+----------+--------------------------------------+----------------------------------------------------------------------------+
   | amprocnum       | Smallint | N/A                                  | Support procedure number                                                   |
   +-----------------+----------+--------------------------------------+----------------------------------------------------------------------------+
   | amproc          | regproc  | :ref:`PG_PROC <dws_04_0608>`.oid     | OID of the procedure                                                       |
   +-----------------+----------+--------------------------------------+----------------------------------------------------------------------------+

The usual interpretation of the **amproclefttype** and **amprocrighttype** columns is that they identify the left and right input types of the operator(s) that a particular support procedure supports. For some access methods these match the input data type(s) of the support procedure itself, for others not. There is a notion of "default" support procedures for an index, which are those with **amproclefttype** and **amprocrighttype** both equal to the index opclass's **opcintype**.
