:original_name: dws_04_0914.html

.. _dws_04_0914:

Logging Destination
===================

log_statement_length_limit
--------------------------

**Parameter description**: Specifies the length of SQL statements to be printed. If the length of an SQL statement exceeds the specified value, the SQL statement is recorded in the **statement-**\ *Timestamp*\ **.log** file. This parameter is supported only by 9.1.0.200 and later versions.

**Type**: SUSET

**Value range**: an integer ranging from 0 to INT_MAX.

**Default value**: **1024**
