:original_name: dws_04_0570.html

.. _dws_04_0570:

PG_AMOP
=======

**PG_AMOP** records information about operators associated with access method operator families. There is one row for each operator that is a member of an operator family. A family member can be either a search operator or an ordering operator. An operator can appear in more than one family, but cannot appear in more than one search position nor more than one ordering position within a family.

.. table:: **Table 1** PG_AMOP columns

   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Name           | Type     | Reference                            | Description                                                                                                 |
   +================+==========+======================================+=============================================================================================================+
   | oid            | oid      | ``-``                                | Row identifier (hidden attribute; must be explicitly selected)                                              |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amopfamily     | oid      | :ref:`PG_OPFAMILY <dws_04_0605>`.oid | Operator family this entry is for                                                                           |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amoplefttype   | oid      | :ref:`PG_TYPE <dws_04_0629>`.oid     | Left-hand input data type of operator                                                                       |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amoprighttype  | oid      | :ref:`PG_TYPE <dws_04_0629>`.oid     | Right-hand input data type of operator                                                                      |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amopstrategy   | smallint | ``-``                                | Number of operator strategies                                                                               |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amoppurpose    | "char"   | ``-``                                | Operator purpose, either **s** for search or **o** for ordering                                             |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amopopr        | oid      | :ref:`PG_OPERATOR <dws_04_0604>`.oid | OID of the operator                                                                                         |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amopmethod     | oid      | :ref:`PG_AM <dws_04_0569>`.oid       | Index access method the operator family is for                                                              |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | amopsortfamily | oid      | :ref:`PG_OPFAMILY <dws_04_0605>`.oid | The btree operator family this entry sorts according to, if an ordering operator; zero if a search operator |
   +----------------+----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+

A "search" operator entry indicates that an index of this operator family can be searched to find all rows satisfying **WHERE indexed_column operator constant**. Obviously, such an operator must return a Boolean value, and its left-hand input type must match the index's column data type.

An "ordering" operator entry indicates that an index of this operator family can be scanned to return rows in the order represented by **ORDER BY indexed_column operator constant**. Such an operator could return any sortable data type, though again its left-hand input type must match the index's column data type. The exact semantics of the **ORDER BY** are specified by the **amopsortfamily** column, which must reference a btree operator family for the operator's result type.
