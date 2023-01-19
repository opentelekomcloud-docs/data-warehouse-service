:original_name: dws_04_0615.html

.. _dws_04_0615:

PG_SECLABEL
===========

**PG_SECLABEL** records security labels on database objects.

See also :ref:`PG_SHSECLABEL <dws_04_0618>`, which performs a similar function for security labels of database objects that are shared across a database cluster.

.. table:: **Table 1** PG_SECLABEL columns

   +----------+---------+-----------------------------------+--------------------------------------------------------------------+
   | Name     | Type    | Reference                         | Description                                                        |
   +==========+=========+===================================+====================================================================+
   | objoid   | oid     | Any OID column                    | OID of the object this security label pertains to                  |
   +----------+---------+-----------------------------------+--------------------------------------------------------------------+
   | classoid | oid     | :ref:`PG_CLASS <dws_04_0578>`.oid | OID of the system catalog that contains the object                 |
   +----------+---------+-----------------------------------+--------------------------------------------------------------------+
   | objsubid | integer | ``-``                             | For a security label on a table column, this is the column number. |
   +----------+---------+-----------------------------------+--------------------------------------------------------------------+
   | provider | text    | ``-``                             | Label provider associated with this label                          |
   +----------+---------+-----------------------------------+--------------------------------------------------------------------+
   | label    | text    | ``-``                             | Security label applied to this object                              |
   +----------+---------+-----------------------------------+--------------------------------------------------------------------+
