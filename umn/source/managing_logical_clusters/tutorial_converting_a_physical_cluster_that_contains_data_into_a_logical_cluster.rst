:original_name: dws_01_7251.html

.. _dws_01_7251:

Tutorial: Converting a Physical Cluster That Contains Data into a Logical Cluster
=================================================================================

Scenario
--------

A large database cluster usually contains a large amount of data put in different tables. With the resource management feature, you can create resource pools to isolate the resources of different services. Different service users can be allocated to different resource pools to reduce resource (CPU, memory, I/O, and storage) competition between services.

As the service scale grows, the number of services in the cluster system also increases. Creating multiple resource pools becomes less effective in controlling resource competition. GaussDB(DWS) uses the distributed architecture. and its data is distributed on multiple nodes. Each table is distributed across all DNs in the cluster, an operation on a data table may involve all DNs, which increases network loads and system resource consumption. To solve this problem, scale-out is not effective. You are advised to divide a GaussDB(DWS) cluster into multiple logical clusters.

You can create a separate logical cluster and assign new services to it. This way, new services have little impact on existing services. Also, if the service scale in existing logical clusters grows, you can scale out the existing logical clusters.

:ref:`Figure 1 <en-us_topic_0000001707293985__en-us_topic_0000001566787932_fig102761610113915>` shows an example. The original service data tables of a company are stored in the original physical cluster **dws-demo** (in green). After services are switched over to the logical cluster **lc1** (in blue), a new logical cluster **lc2** is added to the physical cluster through scale-out. The original service data tables are switched to logical cluster **lc1**, and new service data tables are written to logical cluster **lc2**. In this way, the data of old and new services is isolated. User **u2** associated with logical cluster **lc2** can access the tables of logical cluster **lc1** across logical clusters after authorization.

-  **Cluster scale**: Scale out the original physical cluster from three nodes to six nodes and split it into two logical clusters.
-  **Service isolation**: New and old service data is isolated in different logical clusters.

.. _en-us_topic_0000001707293985__en-us_topic_0000001566787932_fig102761610113915:

.. figure:: /_static/images/en-us_image_0000001711661644.png
   :alt: **Figure 1** Accessing data across logical clusters

   **Figure 1** Accessing data across logical clusters

Creating a Cluster and Preparing Table Data
-------------------------------------------

#. Create a cluster. For details, see :ref:`Creating a Cluster <dws_01_0019>`.

#. After connecting to the database, create table **t1** as the system administrator **dbadmin** and insert two data records into the table.

   ::

      CREATE TABLE t1 (id int, name varchar(20));
      INSERT INTO t1 VALUES (1,'joy'),(2,'lily');

Converting to Logical Cluster lc1
---------------------------------

.. important::

   During the conversion, you can run simple DML statements, such as adding, deleting, modifying, and querying data. Complex DDL statements, such as operations on database objects, will block services. You are advised to perform the conversion during off-peak hours.

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Clusters > Dedicated Cluster**. Click the name of a cluster to go to the **Cluster Information** page.

#. Toggle on the **Logical Cluster** switch.


   .. figure:: /_static/images/en-us_image_0000001711821148.png
      :alt: **Figure 2** Enabling the logical cluster function

      **Figure 2** Enabling the logical cluster function

#. In the navigation pane, choose **Logical Clusters**. Click **Add Logical Cluster** in the upper right corner, enter the logical cluster name **lc1**, and click **OK**.

   During the switchover, the current cluster is unavailable. Wait for about 2 minutes (the conversion time varies depending on the service data volume). If **lc1** is displayed on the logical cluster page, the conversion is successful.


   .. figure:: /_static/images/en-us_image_0000001759420733.png
      :alt: **Figure 3** Adding a logical cluster

      **Figure 3** Adding a logical cluster


   .. figure:: /_static/images/en-us_image_0000001711661648.png
      :alt: **Figure 4** Logical cluster conversion succeeded

      **Figure 4** Logical cluster conversion succeeded

Adding Nodes to the elastic_group Cluster
-----------------------------------------

#. Return to the **Cluster Management** page. In the **Operation** column of the cluster, choose **More** > **Scale Node** > **Scale Out**.


   .. figure:: /_static/images/en-us_image_0000001759580585.png
      :alt: **Figure 5** Scaling out a cluster

      **Figure 5** Scaling out a cluster

#. Set **New Nodes** to **3**. Enable **Online Scale-out**. Set **elastic_group** as the target logical cluster. Confirm the settings, select the confirmation check box, and click **Next: Confirm**.


   .. figure:: /_static/images/en-us_image_0000001759420737.png
      :alt: **Figure 6** Scale-out process

      **Figure 6** Scale-out process

#. Click **Next: Confirm**, and then click **OK**.

   Wait for about 10 minutes until the scale-out is successful.

Adding Logical Cluster lc2
--------------------------

#. On the **Cluster Management** page, click the name of a cluster to go to the cluster details page. In the navigation pane, choose **Logical Clusters**.

#. Click **Add Logical Cluster** in the upper right corner, select three nodes from the right pane to add to the left pane, enter the logical cluster name **lc2**, and click **OK**.

   After about 2 minutes, the logical cluster is successfully added.


   .. figure:: /_static/images/en-us_image_0000001711661652.png
      :alt: **Figure 7** Adding a logical cluster

      **Figure 7** Adding a logical cluster


   .. figure:: /_static/images/en-us_image_0000001759580589.png
      :alt: **Figure 8** Selecting a host ring

      **Figure 8** Selecting a host ring


   .. figure:: /_static/images/en-us_image_0000001711821156.png
      :alt: **Figure 9** Logical cluster added

      **Figure 9** Logical cluster added

Creating Logical Clusters, Associating Them with Users, and Querying Data Across Logical Clusters
-------------------------------------------------------------------------------------------------

#. Connect to the database as the system administrator and run the following SQL statement to query the original service table **t1**:

   Verify that service data can be queried after the conversion.

   ::

      SELECT * FROM t1;

#. Run the following statements to associate **u1** with logical cluster **lc1** and **u2** with logical cluster **lc2**, and grant all permissions of the original service table **t1** to user **u1**:

   ::

      CREATE USER u1 NODE GROUP 'lc1' password '{password}';
      CREATE USER u2 NODE GROUP 'lc2' password '{password}';
      GRANT ALL ON TABLE t1 TO u1;

#. Switch to user **u2** and query data in the original service table **t1**. A message is displayed, indicating that you do not have the permission to access logical cluster **lc1**. This indicates data is isolated between logical clusters.

   ::

      SET ROLE u2 PASSWORD '{password}';
      SELECT * FROM t1;

   |image1|

#. Switch back to system administrator **dbadmin** and grant the access permission of logical cluster **lc1** to user **u2**.

   ::

      SET ROLE dbadmin PASSWORD '{password}';
      GRANT USAGE ON NODE GROUP lc1 TO u2;

#. Switch to user **u2** and query the **t1** table. This proves that the user bound to logical cluster **lc2** can query the original service table **t1** across logical clusters. In this way, data is shared between logical clusters.

   ::

      SET ROLE u2 PASSWORD '{password}';
      SELECT * FROM t1;

   |image2|

.. |image1| image:: /_static/images/en-us_image_0000001711661656.png
.. |image2| image:: /_static/images/en-us_image_0000001759580593.png
