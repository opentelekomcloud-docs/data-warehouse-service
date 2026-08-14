:original_name: dws_04_0624.html

.. _dws_04_0624:

PG_TS_CONFIG
============

**PG_TS_CONFIG** records entries representing text search configurations. A configuration specifies a particular text search parser and a list of dictionaries to use for each of the parser's output token types.

The parser is shown in the **PG_TS_CONFIG** entry, but the token-to-dictionary mapping is defined by subsidiary entries in :ref:`PG_TS_CONFIG_MAP <dws_04_0625>`.

.. table:: **Table 1** PG_TS_CONFIG columns

   +--------------+--------+---------------------------------------+----------------------------------------------------------------------------+
   | Name         | Type   | Reference                             | Description                                                                |
   +==============+========+=======================================+============================================================================+
   | OID          | OID    | N/A                                   | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------------------+
   | cfgname      | Name   | N/A                                   | Text search configuration name                                             |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------------------+
   | cfgnamespace | OID    | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | OID of the namespace where the configuration resides                       |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------------------+
   | cfgowner     | OID    | :ref:`PG_AUTHID <dws_04_0574>`.oid    | Owner of the configuration                                                 |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------------------+
   | cfgparser    | OID    | :ref:`PG_TS_PARSER <dws_04_0627>`.oid | OID of the text search parser for this configuration                       |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------------------+
   | cfoptions    | Text[] | N/A                                   | Configuration options                                                      |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------------------+
