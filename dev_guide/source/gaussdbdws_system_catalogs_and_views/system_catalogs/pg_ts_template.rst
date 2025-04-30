:original_name: dws_04_0628.html

.. _dws_04_0628:

PG_TS_TEMPLATE
==============

**PG_TS_TEMPLATE** records entries defining text search templates. A template provides a framework for text search dictionaries. Since a template must be implemented by C functions, templates can be created only by database administrators.

.. table:: **Table 1** PG_TS_TEMPLATE columns

   +---------------+---------+---------------------------------------+----------------------------------------------------------------+
   | Name          | Type    | Reference                             | Description                                                    |
   +===============+=========+=======================================+================================================================+
   | OID           | OID     | ``-``                                 | Row identifier (hidden attribute; must be explicitly selected) |
   +---------------+---------+---------------------------------------+----------------------------------------------------------------+
   | tmplname      | Name    | ``-``                                 | Text search template name                                      |
   +---------------+---------+---------------------------------------+----------------------------------------------------------------+
   | tmplnamespace | OID     | :ref:`PG_NAMESPACE <dws_04_0600>`.oid | OID of the namespace that contains the template                |
   +---------------+---------+---------------------------------------+----------------------------------------------------------------+
   | tmplinit      | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the template's initialization function                  |
   +---------------+---------+---------------------------------------+----------------------------------------------------------------+
   | tmpllexize    | regproc | :ref:`PG_PROC <dws_04_0608>`.oid      | OID of the template's lexize function                          |
   +---------------+---------+---------------------------------------+----------------------------------------------------------------+
