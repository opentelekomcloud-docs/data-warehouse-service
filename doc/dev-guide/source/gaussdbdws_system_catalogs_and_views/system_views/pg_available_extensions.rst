:original_name: dws_04_0719.html

.. _dws_04_0719:

PG_AVAILABLE_EXTENSIONS
=======================

**PG_AVAILABLE_EXTENSIONS** displays the extended information about certain database features.

.. table:: **Table 1** PG_AVAILABLE_EXTENSIONS columns

   +-------------------+------+-------------------------------------------------------------------------------------------------+
   | Column            | Type | Description                                                                                     |
   +===================+======+=================================================================================================+
   | Name              | Name | Extension name.                                                                                 |
   +-------------------+------+-------------------------------------------------------------------------------------------------+
   | default_version   | Text | Name of default version. The value is **NULL** if none is specified.                            |
   +-------------------+------+-------------------------------------------------------------------------------------------------+
   | installed_version | Text | Currently installed version of the extension. The value is **NULL** if no version is installed. |
   +-------------------+------+-------------------------------------------------------------------------------------------------+
   | comment           | Text | Comment string from the extension's control file.                                               |
   +-------------------+------+-------------------------------------------------------------------------------------------------+
