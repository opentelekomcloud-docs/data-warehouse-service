:original_name: dws_04_1059.html

.. _dws_04_1059:

SHOW_TSC_INFO
=============

Queries TSC information about the current node. This view is supported only by clusters of version 8.2.1 or later.

.. table:: **Table 1** Parameter

   +-----------------------+---------+---------------------------------------------------------------------+
   | Name                  | Type    | Description                                                         |
   +=======================+=========+=====================================================================+
   | node_name             | text    | Node name                                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_mult              | bigint  | TSC conversion multiplier                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_shift             | bigint  | TSC conversion shifts                                               |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_frequency         | float8  | TSC frequency.                                                      |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_use_frequency     | boolean | Indicates whether to use the TSC frequency for time conversion.     |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_ready             | boolean | Indicates whether the TSC frequency can be used for time conversion |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_scalar_error_info | text    | Error information about obtaining TSC conversion information        |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_freq_error_info   | text    | Error information about obtaining TSC frequency information         |
   +-----------------------+---------+---------------------------------------------------------------------+
