:original_name: dws_04_1058.html

.. _dws_04_1058:

GET_TSC_INFO
============

Obtains the TSC information of the current node again. This view is supported only by clusters of version 8.2.1 or later.

.. table:: **Table 1** show_tsc_info() return columns

   +-----------------------+---------+---------------------------------------------------------------------+
   | Column                | Type    | Description                                                         |
   +=======================+=========+=====================================================================+
   | node_name             | text    | Node name                                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_mult              | bigint  | TSC conversion multiplier                                           |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_shift             | bigint  | TSC conversion shifts                                               |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_frequency         | float8  | TSC frequency                                                       |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_use_frequency     | boolean | Indicates whether to use the TSC frequency for time conversion.     |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_ready             | boolean | Indicates whether the TSC frequency can be used for time conversion |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_scalar_error_info | text    | Error information about obtaining TSC conversion information        |
   +-----------------------+---------+---------------------------------------------------------------------+
   | tsc_freq_error_info   | text    | Error information about obtaining TSC frequency information         |
   +-----------------------+---------+---------------------------------------------------------------------+
