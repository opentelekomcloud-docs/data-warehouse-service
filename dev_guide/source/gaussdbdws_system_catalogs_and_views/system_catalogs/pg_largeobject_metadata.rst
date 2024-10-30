:original_name: dws_04_0599.html

.. _dws_04_0599:

PG_LARGEOBJECT_METADATA
=======================

**PG_LARGEOBJECT_METADATA** records metadata associated with large objects. The actual large object data is stored in **PG_LARGEOBJECT**.

.. table:: **Table 1** PG_LARGEOBJECT_METADATA columns

   +----------+-----------+------------------------------------+----------------------------------------------------------------+
   | Name     | Type      | Reference                          | Description                                                    |
   +==========+===========+====================================+================================================================+
   | oid      | oid       | ``-``                              | Row identifier (hidden attribute; must be explicitly selected) |
   +----------+-----------+------------------------------------+----------------------------------------------------------------+
   | lomowner | oid       | :ref:`PG_AUTHID <dws_04_0574>`.oid | Owner of the large object                                      |
   +----------+-----------+------------------------------------+----------------------------------------------------------------+
   | lomacl   | aclitem[] | ``-``                              | Access permissions                                             |
   +----------+-----------+------------------------------------+----------------------------------------------------------------+
