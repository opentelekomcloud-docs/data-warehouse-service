:original_name: dws_03_0196.html

.. _dws_03_0196:

How Do I Grant Schema Permissions to a User?
============================================

This section describes how to grant the query permission for a schema as an example. For more information, see "How Do I Grant Table Permissions to a User?" in FAQ. You can grant:

-  Permission for a table in a schema
-  Permission for all the tables in a schema
-  Permission for tables to be created in the schema

Assume that there are users **u1** and **u2**, and two schemas named after them. User **u2** needs to access tables in schema **u1**.

|image1|

#. Connect to your database as **dbadmin**. Run the following statements to create users **u1** and **u2**. Two schemas will be created and named after the users by default.

   ::

      CREATE USER u1 PASSWORD '{password}';
      CREATE USER u2 PASSWORD '{password}';

#. Create tables **u1.t1** and **u1.t2** in schema **u1**.

   ::

      CREATE TABLE u1.t1 (c1 int, c2 int);
      CREATE TABLE u1.t2 (c1 int, c2 int);

#. Grant the access permission of schema **u1** to user **u2**.

   ::

      GRANT USAGE ON SCHEMA u1 TO u2;

#. .. _en-us_topic_0000001239662887__li185386414379:

   Grant user **u2** the permission to query table **u1.t1** in schema **u1**.

   ::

      GRANT SELECT ON u1.t1 TO u2;

#. Start a new session and connect to the database as user **u2** Verify that user **u2** can query the **u1.t1** table but not the **u1.t2** table.

   ::

      SELECT * FROM u1.t1;
      SELECT * FROM u1.t2;

   |image2|

#. In the session started by user **dbadmin**, grant user **u2** the permission to query all the tables in schema **u1**.

   ::

      GRANT SELECT ON ALL TABLES IN SCHEMA u1 TO u2;

#. In the session started by user **u2**, verify that **u2** can query all tables.

   ::

      SELECT * FROM u1.t1;
      SELECT * FROM u1.t2;

   |image3|

#. In the session started by user **dbadmin**, create table **u1.t3**.

   ::

      CREATE TABLE u1.t3 (c1 int, c2 int);

#. In the session started by user **u2**, verify that user **u2** does not have the query permission for **u1.t3**. It indicates that user **u2** has the permission to access all the existing tables in schema **u1**, but not the tables to be created in the future.

   ::

      SELECT * FROM u1.t3;

   |image4|

#. In the session started by user **dbadmin**, grant user **u2** the permission to query the tables to be created in schema **u1**. Create table **u1.t4**.

   ::

      ALTER DEFAULT PRIVILEGES FOR ROLE u1 IN SCHEMA u1 GRANT SELECT ON TABLES TO u2;
      CREATE TABLE u1.t4 (c1 int, c2 int);

#. In the session started by user **u2**, verify that user **u2** can access table **u1.t4**, but does not have the permission to access **u1.t3**. To let the user access table **u1.t3**, you can grant permissions by performing :ref:`4 <en-us_topic_0000001239662887__li185386414379>`.

   ::

      SELECT * FROM u1.t4;

   |image5|

.. |image1| image:: /_static/images/en-us_image_0000001318546125.png
.. |image2| image:: /_static/images/en-us_image_0000001318103277.png
.. |image3| image:: /_static/images/en-us_image_0000001318263369.png
.. |image4| image:: /_static/images/en-us_image_0000001318423893.png
.. |image5| image:: /_static/images/en-us_image_0000001268864780.png
