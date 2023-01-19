:original_name: dws_04_0607.html

.. _dws_04_0607:

PG_PLTEMPLATE
=============

**PG_PLTEMPLATE** records template information for procedural languages.

.. table:: **Table 1** PG_PLTEMPLATE columns

   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | Name          | Type      | Description                                                                                     |
   +===============+===========+=================================================================================================+
   | tmplname      | name      | Name of the language for which this template is used                                            |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | tmpltrusted   | boolean   | The value is **true** if the language is considered trusted.                                    |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | tmpldbacreate | boolean   | The value is **true** if the language is created by the owner of the database.                  |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | tmplhandler   | text      | Name of the call handler function                                                               |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | tmplinline    | text      | Name of the anonymous block handler. If no name of the block handler exists, the value is null. |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | tmplvalidator | text      | Name of the verification function. If no verification function is available, the value is null. |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | tmpllibrary   | text      | Path of the shared library that implements languages                                            |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
   | tmplacl       | aclitem[] | Access permissions for template (not yet used)                                                  |
   +---------------+-----------+-------------------------------------------------------------------------------------------------+
