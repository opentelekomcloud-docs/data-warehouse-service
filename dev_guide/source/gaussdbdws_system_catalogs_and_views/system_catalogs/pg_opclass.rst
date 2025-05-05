:original_name: dws_04_0603.html

.. _dws_04_0603:

PG_OPCLASS
==========

**PG_OPCLASS** defines index access method operator classes.

Each operator class defines semantics for index columns of a particular data type and a particular index access method. An operator class essentially specifies that a particular operator family is applicable to a particular indexable column data type. The set of operators from the family that are actually usable with the indexed column are whichever ones accept the column's data type as their lefthand input.

.. table:: **Table 1** PG_OPCLASS columns

   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | Name         | Type    | Reference                             | Description                                                                                   |
   +==============+=========+=======================================+===============================================================================================+
   | OID          | OID     | ``-``                                 | Row identifier (hidden attribute; must be explicitly selected)                                |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opcmethod    | OID     | :ref:`PG_AM <dws_04_0569>`.oid        | Index access method the operator class is for                                                 |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opcname      | Name    | ``-``                                 | Name of the operator class                                                                    |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opcnamespace | OID     | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | Namespace to which the operator class belongs                                                 |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opcowner     | OID     | :ref:`PG_AUTHID <dws_04_0574>`.oid    | Owner of the operator class                                                                   |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opcfamily    | OID     | :ref:`PG_OPFAMILY <dws_04_0605>`.oid  | Operator family containing the operator class                                                 |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opcintype    | OID     | :ref:`PG_TYPE <dws_04_0629>`.oid      | Data type that the operator class indexes                                                     |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opcdefault   | boolean | ``-``                                 | Whether the operator class is the default for **opcintype**. If it is, its value is **true**. |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+
   | opckeytype   | OID     | :ref:`PG_TYPE <dws_04_0629>`.oid      | Type of data stored in index, or zero if same as **opcintype**                                |
   +--------------+---------+---------------------------------------+-----------------------------------------------------------------------------------------------+

An operator class's **opcmethod** must match the **opfmethod** of its containing operator family. Also, there must be no more than one **pg_opclass** row having **opcdefault** true for any given combination of **opcmethod** and **opcintype**.
