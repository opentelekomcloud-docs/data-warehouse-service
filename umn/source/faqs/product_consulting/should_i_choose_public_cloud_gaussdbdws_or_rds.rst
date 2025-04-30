:original_name: dws_03_0009.html

.. _dws_03_0009:

Should I Choose Public Cloud GaussDB(DWS) or RDS?
=================================================

Both allow you to run traditional relational databases on the cloud and offload database management tasks. RDS databases are useful for OLTP, reporting, and analysis, but are less capable of handling read operations of a large amount of data (complex read-only queries). GaussDB(DWS) significantly reduces OLAP analysis and reporting workloads on large datasets by up to 10 times through its scalable architecture and optimized algorithms, including **column storage, vectorized executors, and distributed frameworks**.

You can scale out a GaussDB(DWS) cluster to address complex data and queries, or to handle overwhelming analysis and report workloads that affect OLTP performance.

The following table compares OLTP with OLAP.

.. table:: **Table 1** Comparison between OLTP and OLAP

   +---------------+--------------------------------------------------+---------------------------------------------------+
   | Feature       | OLTP                                             | OLAP                                              |
   +===============+==================================================+===================================================+
   | Users         | Operation personnel and junior managers          | Decision-making personnel and senior managers     |
   +---------------+--------------------------------------------------+---------------------------------------------------+
   | Functionality | Daily operations                                 | Analysis and decision-making                      |
   +---------------+--------------------------------------------------+---------------------------------------------------+
   | Design        | Application-oriented                             | Subject-oriented                                  |
   +---------------+--------------------------------------------------+---------------------------------------------------+
   | Data          | Latest, detailed, two-dimensional, and separated | Historical, integrated, multidimensional, unified |
   +---------------+--------------------------------------------------+---------------------------------------------------+
   | Access        | Reads/Writes dozens of records.                  | Reads millions of records.                        |
   +---------------+--------------------------------------------------+---------------------------------------------------+
   | Scope of Work | Simple read/write operations                     | Complex queries                                   |
   +---------------+--------------------------------------------------+---------------------------------------------------+
   | Database size | Hundreds of GB                                   | TB to PB                                          |
   +---------------+--------------------------------------------------+---------------------------------------------------+
