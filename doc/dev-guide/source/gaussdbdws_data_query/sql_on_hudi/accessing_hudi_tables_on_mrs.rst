:original_name: dws_04_1213.html

.. _dws_04_1213:

Accessing Hudi Tables on MRS
============================

SQL on Hudi supports access to hudi tables stored on MRS. This function is supported only by clusters of version 9.1.0 or later.

Prerequisites
-------------

You have created an MRS data source. For details, see "MRS Data Sources" in *Data Warehouse Service (DWS) User Guide*.

.. note::

   SQL on Hudi can read hudi tables stored on MRS. The only difference in usage compared to OBS is when creating data sources.

Accessing Multiple MRS Clusters Concurrently
--------------------------------------------

Due to JDK restrictions, one JVM can store only one Kerberos configuration file at a time. As a result, one GaussDB(DWS) cluster cannot concurrently access hudi tables in multiple MRS clusters through SQL on Hudi. To avoid this issue, do as follows:

#. Obtain the krb5.conf file of each MRS cluster from the downloaded client.

#. Use the krb5.conf file of any MRS cluster as the file to be combined (cluster A for short).

#. Add the KDC domain information of cluster B to **realms** in cluster A's configuration file.

   Example:

   .. code-block::

      [realms]
      CLUSTER.A.COM = {
      admin_server =  ClusterA_SERVER_IP:PORT
      kdc = ClusterA_KDC_IP:PORT
      kdc = ClusterA_KDC_IP:PORT
      }
      CLUSTER.B.COM = {
      admin_server = ClusterB_SERVER_IP:PORT
      kdc = ClusterB_KDC_IP:PORT
      kdc = ClusterB_KDC_IP:PORT
      }

#. Add the domain information of cluster B to **domain_realm** in cluster A's configuration file.

   Example:

   .. code-block::

      [domain_realm]
      .cluster.a.com = CLUSTER.A.COM
      .cluster.b.com = CLUSTER.B.COM

#. Replace the original **krb5.conf** file with the combined one in the original path of each node in each cluster.

.. caution::

   The preceding example is for reference only. During actual operations, you need to combine the actual KDC domain information in the cluster' **realms** or **domain_realm**.
