:original_name: dws_04_0625.html

.. _dws_04_0625:

PG_TS_CONFIG_MAP
================

**PG_TS_CONFIG_MAP** records entries showing which text search dictionaries should be consulted, and in what order, for each output token type of each text search configuration's parser.

.. table:: **Table 1** PG_TS_CONFIG_MAP columns

   +--------------+---------+---------------------------------------+--------------------------------------------------------------------------+
   | Name         | Type    | Reference                             | Description                                                              |
   +==============+=========+=======================================+==========================================================================+
   | mapcfg       | OID     | :ref:`PG_TS_CONFIG <dws_04_0624>`.oid | OID of the :ref:`PG_TS_CONFIG <dws_04_0624>` entry owning this map entry |
   +--------------+---------+---------------------------------------+--------------------------------------------------------------------------+
   | maptokentype | Integer | N/A                                   | A token type emitted by the configuration's parser                       |
   +--------------+---------+---------------------------------------+--------------------------------------------------------------------------+
   | mapseqno     | Integer | N/A                                   | Order in which to consult this entry                                     |
   +--------------+---------+---------------------------------------+--------------------------------------------------------------------------+
   | mapdict      | OID     | :ref:`PG_TS_DICT <dws_04_0626>`.oid   | OID of the text search dictionary to consult                             |
   +--------------+---------+---------------------------------------+--------------------------------------------------------------------------+
