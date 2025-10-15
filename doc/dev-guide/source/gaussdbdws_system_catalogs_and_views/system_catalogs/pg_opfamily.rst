:original_name: dws_04_0605.html

.. _dws_04_0605:

PG_OPFAMILY
===========

**PG_OPFAMILY** defines operator families.

Each operator family is a collection of operators and associated support routines that implement the semantics specified for a particular index access method. Furthermore, the operators in a family are all "compatible", in a way that is specified by the access method. The operator family concept allows cross-data-type operators to be used with indexes and to be reasoned about using knowledge of access method semantics.

.. table:: **Table 1** PG_OPFAMILY columns

   +--------------+------+---------------------------------------+----------------------------------------------------------------------------+
   | Name         | Type | Reference                             | Description                                                                |
   +==============+======+=======================================+============================================================================+
   | OID          | OID  | N/A                                   | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +--------------+------+---------------------------------------+----------------------------------------------------------------------------+
   | opfmethod    | OID  | :ref:`PG_AM <dws_04_0569>`.oid        | Index method used by the operator family                                   |
   +--------------+------+---------------------------------------+----------------------------------------------------------------------------+
   | opfname      | Name | N/A                                   | Name of the operator family                                                |
   +--------------+------+---------------------------------------+----------------------------------------------------------------------------+
   | opfnamespace | OID  | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | Namespace of the operator family                                           |
   +--------------+------+---------------------------------------+----------------------------------------------------------------------------+
   | opfowner     | OID  | :ref:`PG_AUTHID <dws_04_0574>`.oid    | Owner of the operator family                                               |
   +--------------+------+---------------------------------------+----------------------------------------------------------------------------+

The majority of the information defining an operator family is not in **PG_OPFAMILY**, but in the associated :ref:`PG_AMOP <dws_04_0570>`, :ref:`PG_AMPROC <dws_04_0571>`, and :ref:`PG_OPCLASS <dws_04_0603>`.
