:original_name: dws_04_0927.html

.. _dws_04_0927:

Other Default Parameters
========================

This section describes the default database loading parameters of the database system.

dynamic_library_path
--------------------

**Parameter description**: Specifies the path for saving the shared database files that are dynamically loaded for data searching. When a dynamically loaded module needs to be opened and the file name specified in the **CREATE FUNCTION** or **LOAD** command does not have a directory component, the system will search this path for the required file.

The value of **dynamic_library_path** must be a list of absolute paths separated by colons (:) or by semi-colons (;) on the Windows OS. The special variable **$libdir** in the beginning of a path will be replaced with the module installation directory provided by GaussDB(DWS). Example:

::

   dynamic_library_path = '/usr/local/lib/postgresql:/opt/testgs/lib:$libdir'

**Type**: SUSET

**Value range**: a string

.. note::

   If the value of this parameter is set to an empty character string, the automatic path search is turned off.

**Default value**: **$libdir**

gin_fuzzy_search_limit
----------------------

**Parameter description**: Specifies the upper limit of the size of the set returned by GIN indexes.

**Type**: USERSET

**Value range**: an integer ranging from 0 to INT_MAX. The value **0** indicates no limit.

**Default value**: **0**
