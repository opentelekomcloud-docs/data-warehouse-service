:original_name: dws_03_0009.html

.. _dws_03_0009:

Should I Choose Public Cloud GaussDB(DWS) or RDS?
=================================================

Both allow you to run conventional relational databases on the cloud and transfer database management loads. RDS databases are useful for OLTP, reporting, and analysis, but are less capable of handling read operations of a large amount of data (complex read-only queries). GaussDB(DWS) is useful for OLAP by reducing analysis and report workloads of large data sets by an order of magnitude, thanks to its multi-node scale and resources and optimized algorithms (**column storage, vectorized executors, and distributed frameworks**).

You can scale out a GaussDB(DWS) cluster to address complex data and queries, or to handle overwhelming analysis and report workloads that affect OLTP performance.

The following table shows the comparison between OLTP and OLAP.

.. table:: **Table 1** Feature comparison between OLTP and OLAP

   +---------------+---------------------------------------------+---------------------------------------------------+
   | Feature       | RDS for OLTP                                | GaussDB(DWS) for OLAP                             |
   +===============+=============================================+===================================================+
   | User          | Operations and low-level management         | Decision-makers and senior management             |
   +---------------+---------------------------------------------+---------------------------------------------------+
   | Function      | Daily operation processing                  | Analysis and decision-making                      |
   +---------------+---------------------------------------------+---------------------------------------------------+
   | Design        | By application                              | By theme                                          |
   +---------------+---------------------------------------------+---------------------------------------------------+
   | Data          | Latest, detailed, two-dimensional, discrete | Historical, integrated, multidimensional, unified |
   +---------------+---------------------------------------------+---------------------------------------------------+
   | Access        | Dozens of read and write records            | Millions of read records                          |
   +---------------+---------------------------------------------+---------------------------------------------------+
   | Coverage      | Simple read/write operations                | Complex queries                                   |
   +---------------+---------------------------------------------+---------------------------------------------------+
   | Database size | Hundreds of GB                              | TB or PB                                          |
   +---------------+---------------------------------------------+---------------------------------------------------+
