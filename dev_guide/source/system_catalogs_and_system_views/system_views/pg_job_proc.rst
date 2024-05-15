:original_name: dws_04_0734.html

.. _dws_04_0734:

PG_JOB_PROC
===========

The **PG_JOB_PROC** view replaces the **PG_JOB_PROC** system catalog in earlier versions and provides forward compatibility with earlier versions. The original **PG_JOB_PROC** and **PG_JOB** system catalogs are merged into the **PG_JOBS** system catalog in the current version. For details about the **PG_JOBS** system catalog, see :ref:`PG_JOBS <dws_04_0596>`.

.. table:: **Table 1** PG_JOB_PROC columns

   ====== ====== ===========
   Name   Type   Description
   ====== ====== ===========
   job_id bigint Job ID
   what   text   Job content
   ====== ====== ===========
