:original_name: dws_01_7252.html

.. _dws_01_7252:

Tutorial: Dividing a New Physical Cluster into Logical Clusters
===============================================================

Scenario
--------

This section describes how to divide a new six-node physical cluster (having no service data) into two logical clusters. If your physical cluster already has service data, perform operations by referring to :ref:`Tutorial: Converting a Physical Cluster That Contains Data into a Logical Cluster <dws_01_7251>`.

Prerequisites
-------------

Create a six-node cluster. For details, see :ref:`Creating a Cluster <dws_01_0019>`.

Dividing a Cluster into Logical Clusters
----------------------------------------

#. On the **Cluster Management** page, click the name of a cluster to go to the cluster details page. In the navigation pane, choose **Logical Clusters**.

#. Click **Add Logical Cluster** in the upper right corner, select a host ring (three nodes) on the right, add it to the list on the left, enter the logical cluster name **lc1**, and click **OK**.

   After about 2 minutes, the logical cluster is added.

#. Repeat the preceding steps to create the second logical cluster **lc2**.

Creating Logical Clusters, Associating Them with Users, and Querying Data Across Logical Clusters
-------------------------------------------------------------------------------------------------

#. Connect to the database as system administrator **dbadmin** and run the following SQL statement to check whether the logical cluster is created:

   ::

      SELECT group_name FROM PGXC_GROUP;

   |image1|

#. Create users **u1** and **u2** and associate them with logical clusters **lc1** and **lc2**, respectively.

   ::

      CREATE USER u1  NODE GROUP "lc1" password '{password}';
      CREATE USER u2  NODE GROUP "lc2" password '{password}';

#. Switch to user **u1**, create table **t1**, and insert data into the table.

   ::

      SET ROLE u1 PASSWORD '{password}';
      CREATE TABLE u1.t1 (id int);
      INSERT INTO u1.t1 VALUES (1),(2);

#. Switch to user **u2**, create table **t2**, and insert data into the table.

   ::

      SET ROLE u2 PASSWORD '{password}';
      CREATE TABLE u2.t2 (id int);
      INSERT INTO u2.t2 VALUES (1),(2);

#. Query the **u1.t1** table as user **u2**. The command output indicates that the user does not have the permission.

   ::

      SELECT * FROM u1.t1;

   |image2|

#. Switch back to the system administrator **dbadmin** and query the **u1.t1** and **u2.t2** tables, which are created in clusters **lc1** and **lc2**, respectively, corresponding to two services. In this way, data is isolated based on logical clusters.

   ::

      SET ROLE dbadmin PASSWORD '{password}';
      SELECT p.oid,relname,pgroup,nodeoids FROM pg_class p LEFT JOIN pgxc_class pg ON p.oid = pg.pcrelid WHERE p.relname = 't1';
      SELECT p.oid,relname,pgroup,nodeoids FROM pg_class p LEFT JOIN pgxc_class pg ON p.oid = pg.pcrelid WHERE p.relname = 't2';

   |image3|

   |image4|

#. Grant user **u2** the permissions to access logical cluster **lc1**, schema **u1**, and table **u1.t1**.

   ::

      GRANT usage ON NODE GROUP lc1 TO u2;
      GRANT usage ON SCHEMA u1 TO u2;
      GRANT select ON TABLE u1.t1 TO u2;

   .. note::

      Logical clusters implement permission isolation (by node groups) based on physical clusters. To let a user access data across logical clusters, you need to grant the logical cluster (node-group layer) permissions, schema permissions, and table permissions to the user in sequence. If no logical cluster permissions are granted, the error message "permission denied for node group xx" will be displayed.

#. Switch to user **u2** and query the **u1.t1** table. The query is successful. The logical cluster implements data isolation and allows cross-logical cluster access after user authorization.

   ::

      SET ROLE u2 PASSWORD '{password}';
      SELECT * FROM u1.t1;

   |image5|

.. |image1| image:: /_static/images/en-us_image_0000001924729000.png
.. |image2| image:: /_static/images/en-us_image_0000001952008477.png
.. |image3| image:: /_static/images/en-us_image_0000001924569628.png
.. |image4| image:: /_static/images/en-us_image_0000001951848701.png
.. |image5| image:: /_static/images/en-us_image_0000001952008469.png
