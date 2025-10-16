:original_name: dws_04_0031.html

.. _dws_04_0031:

GaussDB(DWS) Foreign Table Function Development Specifications
==============================================================

.. _en-us_topic_0000002125650482__en-us_topic_0000002160959045_section7984144034412:

Rule 4.1 Deploying GDS on an Independent Server Outside the GaussDB(DWS) Cluster
--------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   If GDS is deployed in a GaussDB(DWS) cluster, it contends for resources with CNs or DNs in the cluster, which leads to a decline in the performance of both GDS and CNs or DNs.

   **Solution:**

   -  Deploy GDS on an independent server outside the GaussDB(DWS) cluster.
   -  Ensure that the disk capacity of the GDS server and the network bandwidth between the GDS server and the GaussDB(DWS) cluster are planned according to the requirements.

.. _en-us_topic_0000002125650482__en-us_topic_0000002160959045_section171861828194514:

Rule 4.2 Avoiding Concurrent Access to Multiple Collaborative Analysis Foreign Tables Across Clusters
-----------------------------------------------------------------------------------------------------

.. note::

   **Principle description**: When cluster A accesses data in cluster B through collaborative analysis, all DNs in cluster A establish connections and active sessions with the CNs in cluster B.

   **Impact of rule violation:**

   The CNs in cluster B are overloaded. As a result, the number of connections and active sessions exceeds the threshold, and the access is abnormal.

   **Solution:**

   Use foreign tables to access a single table instead of performing concurrent queries on multiple foreign tables. If it is not possible to avoid concurrent queries, calculate and limit the number of concurrent queries based on the number of DNs in cluster A and the normal service volume of cluster B. Additionally, increasing the values of **max_active_statements** and **max_connections** can help solve the problem.
