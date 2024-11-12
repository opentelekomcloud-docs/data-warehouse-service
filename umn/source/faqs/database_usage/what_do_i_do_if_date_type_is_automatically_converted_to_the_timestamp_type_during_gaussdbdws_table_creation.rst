:original_name: dws_03_0186.html

.. _dws_03_0186:

What Do I Do If Date Type Is Automatically Converted to the Timestamp Type During GaussDB(DWS) Table Creation?
==============================================================================================================

When creating a database, you can set the **DBCOMPATIBILITY** parameter to the compatible database type. The value of **DBCOMPATIBILITY** can be **ORA**, **TD**, and **MySQL**, indicating Oracle, Teradata, and MySQL databases, respectively. If this parameter is not specified during database creation, the default value **ORA** is used. In ORA compatibility mode, the date type is automatically converted to timestamp(0). The date type is only supported in the MySQL compatibility mode.

To solve the problem, you need to change the compatibility mode to MySQL. The compatibility mode of an existing database cannot be changed. It can only be specified during creation of the database. GaussDB(DWS) supports the MySQL compatibility mode in cluster version 8.1.1 and later. To configure this mode, run the following commands:

::

   gaussdb=> CREATE DATABASE mydatabase DBCOMPATIBILITY='mysql';
   CREATE DATABASE
   gaussdb=> \c mydatabase
   Non-SSL connection (SSL connection is recommended when requiring high-security)
   You are now connected to database "mydatabase" as user "dbadmin".
   mydatabase=> create table t1(c1 int, c2 date);
   NOTICE: The 'DISTRIBUTE BY' clause is not specified. Using round-robin as the distribution mode by default.
   HINT: Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
   CREATE TABLE

If the problem cannot be solved by changing the compatibility, you can try to change the column type. For example, insert data of the date type as strings into a table. Example:

::

   gaussdb=> CREATE TABLE mytable (a date,b int);
   CREATE TABLE
   gaussdb=> INSERT INTO mytable VALUES(date '12-08-2023',01);
   INSERT 0 1
   gaussdb=> SELECT * FROM mytable;
             a          | b
   ---------------------+---
    2023-12-08 00:00:00 | 1
   (1 row)
   gaussdb=> ALTER TABLE mytable MODIFY a VARCHAR(20);
   ALTER TABLE
   gaussdb=> INSERT INTO mytable VALUES('2023-12-10',02);
   INSERT 0 1
   gaussdb=> SELECT * FROM mytable;
             a          | b
   ---------------------+---
    2023-12-08 00:00:00 | 1
    2023-12-10          | 2
   (2 rows)
