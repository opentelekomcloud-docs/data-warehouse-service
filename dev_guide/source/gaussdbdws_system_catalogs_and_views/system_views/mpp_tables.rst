:original_name: dws_04_0998.html

.. _dws_04_0998:

MPP_TABLES
==========

**MPP_TABLES** displays information about tables in **PGXC_CLASS**.

.. table:: **Table 1** MPP_TABLES columns

   ========== ================ ==========================================
   Name       Type             Description
   ========== ================ ==========================================
   schemaname name             Name of the schema that contains the table
   tablename  name             Name of a table
   tableowner name             Owner of the table
   tablespace name             Tablespace where the table is located.
   pgroup     name             Name of a node cluster.
   nodeoids   oidvector_extend List of distributed table node OIDs
   ========== ================ ==========================================
