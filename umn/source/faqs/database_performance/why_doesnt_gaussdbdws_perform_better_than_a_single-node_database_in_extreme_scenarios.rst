:original_name: dws_03_0036.html

.. _dws_03_0036:

Why Doesn't GaussDB(DWS) Perform Better Than a Single-Node Database in Extreme Scenarios?
=========================================================================================

Due to the MPP architecture limitation in GaussDB(DWS), a few PostgreSQL system methods and functions cannot be pushed down to DNs for execution. As a result, performance bottlenecks occur only on CNs.

**Explanation:**

-  An operation can be executed concurrently only when it is logically a concurrent operation. For example, SUM performed on all DNs concurrently must centralize the final summarization on one CN. In this case, most of the summarization work has been completed on DNs, so the work on the CN is relatively lightweight.
-  In some scenarios, the operation must be executed centrally on one node. For example, assigning a globally unique name to a transaction ID is implemented using the system GTM. Therefore, the GTM is also a globally unique component (active/standby). All globally unique tasks are implemented through the GTM in GaussDB(DWS), but software code is optimized to reduce this kind of tasks. Therefore, the GTM does not have much of a bottleneck. In some scenarios, GTM-Free and GTM-Lite can be implemented.
-  To ensure excellent performance, services need to be slightly modified for adaptation during migration from the application development mode of the traditional single-node database to that of the parallel database, especially for the traditional stored procedure nesting of Oracle.

**Solutions:**

-  If such a problem occurs, see the section about query performance tuning in the *GaussDB(DWS) Developer Guide*.
-  Alternatively, contact technical support to modify and optimize services.
