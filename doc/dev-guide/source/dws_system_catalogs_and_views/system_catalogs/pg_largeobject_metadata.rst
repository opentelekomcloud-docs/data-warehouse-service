:original_name: dws_04_0599.html

.. _dws_04_0599:

PG_LARGEOBJECT_METADATA
=======================

**PG_LARGEOBJECT_METADATA** records metadata associated with large objects. The actual large object data is stored in **PG_LARGEOBJECT**.

.. table:: **Table 1** PG_LARGEOBJECT_METADATA columns

   +----------+-----------+------------------------------------+----------------------------------------------------------------------------+
   | Name     | Type      | Reference                          | Description                                                                |
   +==========+===========+====================================+============================================================================+
   | OID      | OID       | N/A                                | Row identifier (hidden attribute; displayed only when explicitly selected) |
   +----------+-----------+------------------------------------+----------------------------------------------------------------------------+
   | lomowner | OID       | :ref:`PG_AUTHID <dws_04_0574>`.oid | Owner of the large object                                                  |
   +----------+-----------+------------------------------------+----------------------------------------------------------------------------+
   | lomacl   | aclitem[] | N/A                                | Access permissions                                                         |
   +----------+-----------+------------------------------------+----------------------------------------------------------------------------+
