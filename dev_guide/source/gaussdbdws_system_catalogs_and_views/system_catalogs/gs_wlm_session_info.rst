:original_name: dws_04_0566.html

.. _dws_04_0566:

GS_WLM_SESSION_INFO
===================

**GS_WLM_SESSION_INFO** records load management information about a completed job executed on all CNs. The data is dumped from the kernel to a system catalog. If the GUC parameter :ref:`enable_resource_record <en-us_topic_0000001510522653__s5f116e109a2944e3854abcc56772eaa1>` is set to **on**, the system imports records from :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>` to this system catalog every three minutes. You are not advised to enable this function because it occupies storage space and affects performance. For details about the columns, see :ref:`Table 1 <en-us_topic_0000001510283413__tb435fec1dc744bb3872aab277c2a87d8>`.

.. note::

   -  This system catalog's schema is **dbms_om**.
   -  This system catalog has a distribution column, the gaussdb column, in PostgreSQL databases only, not other databases.
   -  The **pg_catalog** has the **GS_WLM_SESSION_INFO** view.
