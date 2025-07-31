:original_name: dws_03_0196.html

.. _dws_03_0196:

How Do I Grant the Permissions of a Schema to a Specified GaussDB(DWS) User?
============================================================================

This section explains how to give query permission for schema-level permissions. If you need other permissions, see "How Do I Grant Table Permissions to a User?" in FAQ.

-  Permission for a table in a schema
-  Permission for all the tables in a schema
-  Permission for tables to be created in the schema

   .. caution::

      The VACUUM, DROP, and ALTER permissions on foreign tables cannot be granted to users.

Assume that there are users **jim** and **mike**, and two schemas named after them. User **mike** needs to access tables in schema **jim**, as shown in :ref:`Figure 1 <en-us_topic_0000001330808760__fig85201744134414>`.

.. _en-us_topic_0000001330808760__fig85201744134414:

.. figure:: /_static/images/en-us_image_0000001936081689.png
   :alt: **Figure 1** User **mike** accesses a table in SCHEMA **jim**.

   **Figure 1** User **mike** accesses a table in SCHEMA **jim**.

#. Connect to your database as **dbadmin**. Run the following statements to create users **jim** and **mike**. Two schemas will be created and named after the users by default.

   ::

      CREATE USER jim PASSWORD '{password}';
      CREATE USER mike PASSWORD '{password}';

#. Create tables **jim.name** and **jim.address** in schema **jim**.

   ::

      CREATE TABLE jim.name (c1 int, c2 int);
      CREATE TABLE jim.address (c1 int, c2 int);

#. Grant the access permission of schema **jim** to user **mike**.

   ::

      GRANT USAGE ON SCHEMA jim TO mike;

#. .. _en-us_topic_0000001330808760__en-us_topic_0000001239662887_li185386414379:

   Grant user **mike** the permission to query table **jim.name** in schema **jim**.

   ::

      GRANT SELECT ON jim.name TO mike;

#. Start a new session and connect to the database as user **mike**. Verify that user **mike** can query the **jim.name** table but not the **jim.address** table.

   ::

      SELECT * FROM jim.name;
      SELECT * FROM jim.address;

   |image1|

#. In the session started by user **dbadmin**, grant user **mike** the permission to query all the tables in schema **jim**.

   ::

      GRANT SELECT ON ALL TABLES IN SCHEMA jim TO mike;

#. In the session started by user **mike**, verify that **mike** can query all tables.

   ::

      SELECT * FROM jim.name;
      SELECT * FROM jim.address;

   |image2|

#. In the session started by user **dbadmin**, create table **jim.employ**.

   ::

      CREATE TABLE jim.employ (c1 int, c2 int);

#. In the session started by user **mike**, verify that user **mike** does not have the query permission for **jim.employ**. It indicates that user **mike** has the permission to access all the existing tables in schema **jim**, but not the tables to be created in the future.

   ::

      SELECT * FROM jim.employ;

   |image3|

#. In the session started by user **dbadmin**, grant user **mike** the permission to query the tables to be created in schema **jim**. Create table **jim.hobby**.

   ::

      ALTER DEFAULT PRIVILEGES FOR ROLE jim IN SCHEMA jim GRANT SELECT ON TABLES TO mike;
      CREATE TABLE jim.hobby (c1 int, c2 int);

#. In the session started by user **mike**, verify that user **mike** can access table **jim.hobby**, but does not have the permission to access **jim.employ**. To let the user access table **jim.employ**, you can grant permissions by performing :ref:`4 <en-us_topic_0000001330808760__en-us_topic_0000001239662887_li185386414379>`.

   ::

      SELECT * FROM jim.hobby;

   |image4|

.. |image1| image:: /_static/images/en-us_image_0000001936107161.png
.. |image2| image:: /_static/images/en-us_image_0000001936108293.png
.. |image3| image:: /_static/images/en-us_image_0000001936109033.png
.. |image4| image:: /_static/images/en-us_image_0000001894150128.png
