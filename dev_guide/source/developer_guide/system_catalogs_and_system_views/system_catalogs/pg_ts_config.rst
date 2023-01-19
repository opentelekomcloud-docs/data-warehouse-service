:original_name: dws_04_0624.html

.. _dws_04_0624:

PG_TS_CONFIG
============

**PG_TS_CONFIG** records entries representing text search configurations. A configuration specifies a particular text search parser and a list of dictionaries to use for each of the parser's output token types.

The parser is shown in the **PG_TS_CONFIG** entry, but the token-to-dictionary mapping is defined by subsidiary entries in :ref:`PG_TS_CONFIG_MAP <dws_04_0625>`.

.. table:: **Table 1** PG_TS_CONFIG columns

   +--------------+--------+---------------------------------------+----------------------------------------------------------------+
   | Name         | Type   | Reference                             | Description                                                    |
   +==============+========+=======================================+================================================================+
   | oid          | oid    | ``-``                                 | Row identifier (hidden attribute; must be explicitly selected) |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------+
   | cfgname      | name   | ``-``                                 | Text search configuration name                                 |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------+
   | cfgnamespace | oid    | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | OID of the namespace where the configuration resides           |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------+
   | cfgowner     | oid    | :ref:`PG_AUTHID <dws_04_0574>`.oid    | Owner of the configuration                                     |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------+
   | cfgparser    | oid    | :ref:`PG_TS_PARSER <dws_04_0627>`.oid | OID of the text search parser for this configuration           |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------+
   | cfoptions    | text[] | ``-``                                 | Configuration options                                          |
   +--------------+--------+---------------------------------------+----------------------------------------------------------------+
