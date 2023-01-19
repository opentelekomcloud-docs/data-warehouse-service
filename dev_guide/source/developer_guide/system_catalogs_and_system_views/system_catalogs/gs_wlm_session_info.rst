:original_name: dws_04_0566.html

.. _dws_04_0566:

GS_WLM_SESSION_INFO
===================

**GS_WLM_SESSION_INFO** records load management information about a completed job executed on all CNs. The data is dumped from the kernel to a system catalog.

.. note::

   -  This system catalog's schema is **dbms_om**.
   -  This system catalog has a distribution column, the gaussdb column, in PostgreSQL databases only, not other databases.
   -  The **pg_catalog** has the **GS_WLM_SESSION_INFO** view.
