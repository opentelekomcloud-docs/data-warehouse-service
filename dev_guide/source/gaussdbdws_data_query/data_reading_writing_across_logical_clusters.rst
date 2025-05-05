:original_name: dws_04_1239.html

.. _dws_04_1239:

Data Reading/Writing Across Logical Clusters
============================================

Scenario
--------

After an associated logical cluster user is created, the query or modification (including Insert, Delete, and Update) submitted by the user is calculated and executed in the associated logical cluster. If the user submits a query or modification request to a table in a different logical cluster, the optimizer generates a cross-logical cluster query or modification plan to enable the user to query or modify the table.


.. figure:: /_static/images/en-us_image_0000001981659785.png
   :alt: **Figure 1** Querying data across logical clusters

   **Figure 1** Querying data across logical clusters


.. figure:: /_static/images/en-us_image_0000001981659793.png
   :alt: **Figure 2** Writing data across logical clusters

   **Figure 2** Writing data across logical clusters

Procedure
---------

#. Create a cluster by referring to section "Creating a Decoupled Storage and Compute Cluster" in the *User Guide*. After a cluster is created, it is converted to a logical cluster **v3_logical** by default

#. Add three nodes to the elastic cluster, and then add the logical cluster **lc2**.

#. Create user **u1** and associate it with logical cluster **v3_logical**.

   ::

      CREATE USER u1 with SYSADMIN NODE GROUP "v3_logical" password  "Password@123";

#. Create user **u2** and associate it with logical cluster **lc2**.

   ::

      CREATE USER u2 with SYSADMIN NODE GROUP "lc2" password  "Password@123";

#. Log in to the database as user **u1**, create tables **t1** and **t2**, and insert test data into the tables.

   ::

      CREATE TABLE  public.t1
      (
      id integer not null,
      data integer,
      age integer
      )
      WITH (ORIENTATION =COLUMN, COLVERSION =3.0)
      DISTRIBUTE BY ROUNDROBIN;

      CREATE TABLE public.t2
      (
      id integer not null,
      data integer,
      age integer
      )
      WITH (ORIENTATION = COLUMN, COLVERSION =3.0)
      DISTRIBUTE BY ROUNDROBIN;

      INSERT INTO public.t1 VALUES (1,2,10),(2,3,11);
      INSERT INTO public.t2 VALUES (1,2,10),(2,3,11);

#. Log in to the database as user **u2** and run the commands below to query **t1** and write data.

   According to the result, user **u2** can query and write data across logical clusters.

   ::

      SELECT * FROM t1;
      INSERT INTO t1 SELECT * FROM t2;
