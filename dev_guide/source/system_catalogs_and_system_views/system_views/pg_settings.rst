:original_name: dws_04_0751.html

.. _dws_04_0751:

PG_SETTINGS
===========

**PG_SETTINGS** displays information about parameters of the running database.

.. table:: **Table 1** PG_SETTINGS columns

   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name       | Type    | Description                                                                                                                                                               |
   +============+=========+===========================================================================================================================================================================+
   | name       | text    | Parameter name                                                                                                                                                            |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | setting    | text    | Current value of the parameter                                                                                                                                            |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | unit       | text    | Implicit unit of the parameter                                                                                                                                            |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | category   | text    | Logical group of the parameter                                                                                                                                            |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | short_desc | text    | Brief description of the parameter                                                                                                                                        |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | extra_desc | text    | Detailed description of the parameter                                                                                                                                     |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | context    | text    | Context of parameter values including internal, postmaster, sighup, backend, superuser, and user                                                                          |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | vartype    | text    | Parameter type. It can be **bool**, **enum**, **integer**, **real**, or **string**.                                                                                       |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | source     | text    | Method of assigning the parameter value                                                                                                                                   |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_val    | text    | Minimum value of the parameter. If the parameter type is not numeric data, the value of this column is null.                                                              |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_val    | text    | Maximum value of the parameter. If the parameter type is not numeric data, the value of this column is null.                                                              |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enumvals   | text[]  | Valid values of an enum-typed parameter. If the parameter type is not enum, the value of this column is null.                                                             |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | boot_val   | text    | Default parameter value used upon the database startup                                                                                                                    |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | reset_val  | text    | Default parameter value used upon the database reset                                                                                                                      |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sourcefile | text    | Configuration file used to set parameter values. If parameter values are not configured using the configuration file, the value of this column is null.                   |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | sourceline | integer | Row number of the configuration file for setting parameter values. If parameter values are not configured using the configuration file, the value of this column is null. |
   +------------+---------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
