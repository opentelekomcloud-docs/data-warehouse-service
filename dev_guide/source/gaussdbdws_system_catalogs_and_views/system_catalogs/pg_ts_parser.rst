:original_name: dws_04_0627.html

.. _dws_04_0627:

PG_TS_PARSER
============

**PG_TS_PARSER** records entries defining text search parsers. A parser splits input text into lexemes and assigns a token type to each lexeme. Since a parser must be implemented by C functions, parsers can be created only by database administrators.

.. table:: **Table 1** PG_TS_PARSER columns

   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | Name         | Type    | Reference                             | Description                                                                |
   +==============+=========+=======================================+============================================================================+
   | OID          | OID     | N/A                                   | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | prsname      | Name    | N/A                                   | Text search parser name                                                    |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | prsnamespace | OID     | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | OID of the namespace that contains the parser                              |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | prsstart     | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the parser's startup function                                       |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | prstoken     | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the parser's next-token function                                    |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | prsend       | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the parser's shutdown function                                      |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | prsheadline  | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the parser's headline function                                      |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
   | prslextype   | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the parser's lextype function                                       |
   +--------------+---------+---------------------------------------+----------------------------------------------------------------------------+
