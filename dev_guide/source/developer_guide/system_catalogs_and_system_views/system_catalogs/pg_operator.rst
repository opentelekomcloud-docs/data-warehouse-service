:original_name: dws_04_0604.html

.. _dws_04_0604:

PG_OPERATOR
===========

**PG_OPERATOR** records information about operators.

.. table:: **Table 1** PG_OPERATOR columns

   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | Name            | Type            | Reference                             | Description                                                    |
   +=================+=================+=======================================+================================================================+
   | oid             | oid             | ``-``                                 | Row identifier (hidden attribute; must be explicitly selected) |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprname         | name            | ``-``                                 | Name of the operator                                           |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprnamespace    | oid             | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | OID of the namespace that contains this operator               |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprowner        | oid             | :ref:`PG_AUTHID <dws_04_0574>`.oid    | Owner of the operator                                          |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprkind         | "char"          | ``-``                                 | -  **b**: infix ("both")                                       |
   |                 |                 |                                       | -  **l**: prefix ("left")                                      |
   |                 |                 |                                       | -  **r**: postfix ("right")                                    |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprcanmerge     | boolean         | ``-``                                 | Whether the operator supports merge joins                      |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprcanhash      | boolean         | ``-``                                 | Whether the operator supports hash joins                       |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprleft         | oid             | :ref:`PG_TYPE <dws_04_0629>`.oid      | Type of the left operand                                       |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprright        | oid             | :ref:`PG_TYPE <dws_04_0629>`.oid      | Type of the right operand                                      |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprresult       | oid             | :ref:`PG_TYPE <dws_04_0629>`.oid      | Type of the result                                             |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprcom          | oid             | :ref:`PG_OPERATOR <dws_04_0604>`.oid  | Commutator of this operator, if any                            |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprnegate       | oid             | :ref:`PG_OPERATOR <dws_04_0604>`.oid  | Negator of this operator, if any                               |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprcode         | regproc         | :ref:`PG_PROC <dws_04_0608>`.oid      | Function that implements this operator                         |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprrest         | regproc         | :ref:`PG_PROC <dws_04_0608>`.oid      | Restriction selectivity estimation function for this operator  |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
   | oprjoin         | regproc         | :ref:`PG_PROC <dws_04_0608>`.oid      | Join selectivity estimation function for this operator         |
   +-----------------+-----------------+---------------------------------------+----------------------------------------------------------------+
