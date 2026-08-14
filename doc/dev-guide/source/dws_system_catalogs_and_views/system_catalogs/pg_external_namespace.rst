:original_name: dws_04_1096.html

.. _dws_04_1096:

PG_EXTERNAL_NAMESPACE
=====================

Stores EXTERNAL SCHEMA information. This system catalog is supported only in 8.3.0 and later versions.

.. table:: **Table 1** PG_EXTERNAL_NAMESPACE columns

   ========== ====== =====================================================
   Name       Type   Description
   ========== ====== =====================================================
   nspid      OID    External schema OID
   srvname    Text   Name of the foreign server
   source     Text   Metadata service type
   address    Text   Metadata service address
   database   Text   Metadata server database
   confpath   Text   Path of the configuration file of the metadata server
   ensoptions Text[] Reserved column, which is left empty currently.
   catalog    Text   Metadata server catalog
   ========== ====== =====================================================

Example
-------

Query the created **EXTERNAL SCHEMA ex1**:

::

   SELECT * FROM pg_external_namespace WHERE nspid = (SELECT oid FROM pg_namespace WHERE nspname = 'ex1');
