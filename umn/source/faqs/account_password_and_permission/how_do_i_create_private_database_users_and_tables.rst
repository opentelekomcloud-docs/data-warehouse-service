:original_name: dws_03_0199.html

.. _dws_03_0199:

How Do I Create Private Database Users and Tables?
==================================================

Scenario
--------

The system administrator **dbadmin** has the permission to access tables created by common users by default. When Separation of Permissions is enabled, the administrator **dbadmin** does not have the permission to access tables of common users or perform control operations (DROP, ALTER, and TRUNCATE).

If a private user and a private table (table created by the private user) need to be created, and the private table can be accessed only by the private user and the system administrator **dbadmin** and other common users do not have the permission to access the table (INSERT, DELETE, UPDATE, SELECT, and COPY). However, the system administrator **dbadmin** sometimes need to perform the DROP, ALTER, or TRUNCATE operations without authorization from the private user. In this case, you can create a user (private user) with the INDEPENDENT attribute.


.. figure:: /_static/images/en-us_image_0000001447013230.png
   :alt: **Figure 1** Private users

   **Figure 1** Private users

Principles
----------

This function is implemented by creating a user with the INDEPENDENT attribute.

**INDEPENDENT \| NOINDEPENDENT** defines private and independent roles. For a role with the **INDEPENDENT** attribute, administrators' rights to control and access this role are separated. Specific rules are as follows:

-  Administrators have no rights to add, delete, query, modify, copy, or authorize the corresponding table objects without the authorization from the INDEPENDENT role.
-  Administrators have no rights to modify the inheritance relationship of the INDEPENDENT role without the authorization from this role.
-  Administrators have no rights to modify the owner of the table objects for the INDEPENDENT role.
-  Administrators have no rights to change the database password of the INDEPENDENT role. The INDEPENDENT role must manage its own password, which cannot be reset if lost.
-  The **SYSADMIN** attribute of a user cannot be changed to the **INDEPENDENT** attribute.

Procedure
---------

#. Connect to the DWS database as user **dbadmin**.

#. Run the following SQL statement to create private user **u1**:

   ::

      CREATE USER u1 WITH INDEPENDENT IDENTIFIED BY 'password';

3. Switch to user **u1**, create the table **test**, and insert data into the table.

   ::

      CREATE TABLE test (id INT, name VARCHAR(20));
      INSERT INTO test VALUES (1, 'joe');
      INSERT INTO test VALUES (2, 'jim');

4. Switch to user **dbadmin** and run the following SQL statement to check whether user **dbadmin** can access the private table **test** created by private user **u1**:

   ::

      SELECT * FROM u1.test;

   The query result indicates that the user **dbadmin** does not have the access permission. This means the private user and private table are created successfully.

   |image1|

5. Run the **DROP** statement as user **dbadmin** to delete the table **test**.

   ::

      DROP TABLE u1.test;

   |image2|

.. |image1| image:: /_static/images/en-us_image_0000001496851569.png
.. |image2| image:: /_static/images/en-us_image_0000001446859452.png
