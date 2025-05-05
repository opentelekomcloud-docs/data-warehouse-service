:original_name: dws_04_0618.html

.. _dws_04_0618:

PG_SHSECLABEL
=============

**PG_SHSECLABEL** records security labels on shared database objects. Security labels can be manipulated with the **SECURITY LABEL** command.

For an easier way to view security labels, see :ref:`PG_SECLABELS <dws_04_0748>`.

See also :ref:`PG_SECLABEL <dws_04_0615>`, which performs a similar function for security labels involving objects within a single database.

Unlike most system catalogs, **PG_SHSECLABEL** is shared across all databases of a cluster. There is only one copy of **PG_SHSECLABEL** per cluster, not one per database.

.. table:: **Table 1** PG_SHSECLABEL columns

   +----------+------+-----------------------------------+----------------------------------------------------+
   | Name     | Type | Reference                         | Description                                        |
   +==========+======+===================================+====================================================+
   | objoid   | OID  | Any OID column                    | OID of the object this security label pertains to  |
   +----------+------+-----------------------------------+----------------------------------------------------+
   | classoid | OID  | :ref:`PG_CLASS <dws_04_0578>`.oid | OID of the system catalog where the object resides |
   +----------+------+-----------------------------------+----------------------------------------------------+
   | provider | Text | N/A                               | Label provider associated with this label          |
   +----------+------+-----------------------------------+----------------------------------------------------+
   | label    | Text | N/A                               | Security label applied to this object              |
   +----------+------+-----------------------------------+----------------------------------------------------+
