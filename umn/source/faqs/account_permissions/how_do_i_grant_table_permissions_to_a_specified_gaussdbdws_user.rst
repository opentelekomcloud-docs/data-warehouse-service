:original_name: dws_03_0197.html

.. _dws_03_0197:

How Do I Grant Table Permissions to a Specified GaussDB(DWS) User?
==================================================================

This section describes how to grant users the SELECT, INSERT, UPDATE, or full permissions of tables to users.

Syntax
------

::

   GRANT { { SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER | ANALYZE | ANALYSE } [, ...]
         | ALL [ PRIVILEGES ] }
       ON { [ TABLE ] table_name [, ...]
          | ALL TABLES IN SCHEMA schema_name [, ...] }
       TO { [ GROUP ] role_name | PUBLIC } [, ...]
       [ WITH GRANT OPTION ];

Scenario
--------

Assume there are users **u1**, **u2**, **u3**, **u4**, and **u5** and five schemas named after these users. Their permission requirements are as follows:

-  User **u2** is a read-only user and requires the SELECT permission for the **u1.t1** table.
-  User **u3** requires the SELECT permission for the **u1.t1** table.
-  User **u3** requires the UPDATE permission for the **u1.t1** table.
-  User **u5** requires all permissions of table **u1.t1**.

|image1|

.. table:: **Table 1** Permissions of the u1.t1 table

   +---------+----------------------------+-----------------------------------------------------------------------------------------------------------------+---------+---------+---------+---------+
   | User    | Type                       | GRANT Statement                                                                                                 | Query   | Insert  | Update  | Delete  |
   +=========+============================+=================================================================================================================+=========+=========+=========+=========+
   | u1      | Owner                      | ``-``                                                                                                           | **Y**   | **Y**   | **Y**   | **Y**   |
   +---------+----------------------------+-----------------------------------------------------------------------------------------------------------------+---------+---------+---------+---------+
   | u2      | Read-only user             | ::                                                                                                              | **Y**   | x       | x       | x       |
   |         |                            |                                                                                                                 |         |         |         |         |
   |         |                            |    GRANT SELECT ON u1.t1 TO u2;                                                                                 |         |         |         |         |
   +---------+----------------------------+-----------------------------------------------------------------------------------------------------------------+---------+---------+---------+---------+
   | u3      | INSERT user                | ::                                                                                                              | x       | **Y**   | x       | x       |
   |         |                            |                                                                                                                 |         |         |         |         |
   |         |                            |    GRANT INSERT ON u1.t1 TO u3;                                                                                 |         |         |         |         |
   +---------+----------------------------+-----------------------------------------------------------------------------------------------------------------+---------+---------+---------+---------+
   | u4      | UPDATE user                | ::                                                                                                              | **Y**   | x       | **Y**   | x       |
   |         |                            |                                                                                                                 |         |         |         |         |
   |         |                            |    GRANT SELECT,UPDATE ON u1.t1 TO u4;                                                                          |         |         |         |         |
   |         |                            |                                                                                                                 |         |         |         |         |
   |         |                            | .. important::                                                                                                  |         |         |         |         |
   |         |                            |                                                                                                                 |         |         |         |         |
   |         |                            |    NOTICE:                                                                                                      |         |         |         |         |
   |         |                            |    The UPDATE permission must be granted together with the SELECT permission, or information leakage may occur. |         |         |         |         |
   +---------+----------------------------+-----------------------------------------------------------------------------------------------------------------+---------+---------+---------+---------+
   | u5      | Users with all permissions | ::                                                                                                              | **Y**   | **Y**   | **Y**   | **Y**   |
   |         |                            |                                                                                                                 |         |         |         |         |
   |         |                            |    GRANT ALL PRIVILEGES ON u1.t1 TO u5;                                                                         |         |         |         |         |
   +---------+----------------------------+-----------------------------------------------------------------------------------------------------------------+---------+---------+---------+---------+

Procedure
---------

Perform the following steps to grant and verify permissions:

#. Connect to your database as **dbadmin**. Run the following statements to create users **u1** to **u5**. Five schemas will be created and named after the users by default.

   ::

      CREATE USER u1 PASSWORD '{password}';
      CREATE USER u2 PASSWORD '{password}';
      CREATE USER u3 PASSWORD '{password}';
      CREATE USER u4 PASSWORD '{password}';
      CREATE USER u5 PASSWORD '{password}';

2.  Create table **u1.t1** in schema **u1**.

    ::

       CREATE TABLE u1.t1 (c1 int, c2 int);

3.  Insert two records to the table.

    ::

       INSERT INTO u1.t1 VALUES (1,2);
       INSERT INTO u1.t1 VALUES (1,2);

4.  Grant schema permissions to users.

    ::

       GRANT USAGE ON SCHEMA u1 TO u2,u3,u4,u5;

5.  Grant user **u2** the permission to query the **u1.t1** table.

    ::

       GRANT SELECT ON u1.t1 TO u2;

6.  Start a new session and connect to the database as user **u2**. Verify that user **u2** can query the **u1.t1** table but cannot write to or modify the table.

    ::

       SELECT * FROM u1.t1;
       INSERT INTO u1.t1 VALUES (1,20);
       UPDATE u1.t1 SET c2 = 3 WHERE c1 =1;

    |image2|

7.  In the session started by user **dbadmin**, grant permissions to users **u3**, **u4**, and **u5**.

    ::

       GRANT INSERT ON u1.t1 TO u3; -- Allow u3 to insert data.
       GRANT SELECT,UPDATE ON u1.t1 TO u4; -- Allow u4 to modify the table.
       GRANT ALL PRIVILEGES ON u1.t1 TO u5; -- Allow u5 to query, insert, modify, and delete table data.

8.  Start a new session and connect to the database as user **u3**. Verify that user **u3** can query the **u1.t1** table but cannot query or modify the table.

    ::

       SELECT * FROM u1.t1;
       INSERT INTO u1.t1 VALUES (1,20);
       UPDATE u1.t1 SET c2 = 3 WHERE c1 =1;

    |image3|

9.  Start a new session and connect to the database as user **u4**. Verify that user **u4** can modify and query the **u1.t1** table, but cannot insert data to the table.

    ::

       SELECT * FROM u1.t1;
       INSERT INTO u1.t1 VALUES (1,20);
       UPDATE u1.t1 SET c2 = 3 WHERE c1 =1;

    |image4|

10. Start a new session and connect to the database as user **u5**. Verify that user **u5** can query, insert, modify, and delete data in the **u1.t1** table.

    ::

       SELECT * FROM u1.t1;
       INSERT INTO u1.t1 VALUES (1,20);
       UPDATE u1.t1 SET c2 = 3 WHERE c1 =1;
       DELETE FROM u1.t1;

    |image5|

11. In the session started by user **dbadmin**, execute the has_table_privilege function to query user permissions.

    ::

       SELECT * FROM pg_class WHERE relname = 't1';

    Check the **relacl** column in the command output. *rolename*\ **=**\ *xxxx/yyyy* indicates that *rolename* has the *xxxx* permission on the table and the permission is obtained from *yyyy*.

    The following figure shows the command output.

    |image6|

    -  **u1=arwdDxtA/u1** indicates that **u1** is the owner and has full permissions.
    -  **u2=r/u1** indicates that **u2** has the read permission.
    -  **u3=a/u1** indicates that **u3** has the insert permission.
    -  **u4=rw/u1** indicates that **u4** has the read and update permissions.
    -  **u5=arwdDxtA/u1** indicates that **u5** has full permissions.

.. |image1| image:: /_static/images/en-us_image_0000001381728629.png
.. |image2| image:: /_static/images/en-us_image_0000001381889117.png
.. |image3| image:: /_static/images/en-us_image_0000001381808801.png
.. |image4| image:: /_static/images/en-us_image_0000001330808808.png
.. |image5| image:: /_static/images/en-us_image_0000001330648836.png
.. |image6| image:: /_static/images/en-us_image_0000001330329232.png
