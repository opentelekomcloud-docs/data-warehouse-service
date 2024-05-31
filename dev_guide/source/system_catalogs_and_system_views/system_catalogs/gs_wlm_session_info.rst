:original_name: dws_04_0566.html

.. _dws_04_0566:

GS_WLM_SESSION_INFO
===================

**GS_WLM_SESSION_INFO** records load management information about a completed job executed on all CNs. The data is dumped from the kernel to a system catalog. If the GUC parameter :ref:`enable_resource_record <en-us_topic_0000001233563121__s5f116e109a2944e3854abcc56772eaa1>` is set to **on**, the system imports records in :ref:`GS_WLM_SESSION_HISTORY <dws_04_0705>` to this system catalog every three minutes. You are not advised to enable this function because it occupies storage space and affects performance. For details about the columns, see :ref:`Table 1 <en-us_topic_0000001233883311__tb435fec1dc744bb3872aab277c2a87d8>`.

.. note::

   -  This system catalog's schema is **dbms_om**.
   -  The **pg_catalog** has the **GS_WLM_SESSION_INFO** view.
