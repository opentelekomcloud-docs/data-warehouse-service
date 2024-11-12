:original_name: dws_03_0080.html

.. _dws_03_0080:

How Do I Implement Fault Tolerance Import Between Different GaussDB(DWS) Encoding Libraries
===========================================================================================

To import data from database A (UTF8) to database B (GBK), there may be a character set mismatch error which causes the data import to fail.

To import a small amount of data, run the **\\COPY** command. The procedure is as follows:

#. Create databases A and B. The encoding format of database A is UTF8, and that of database B is GBK.

   ::

      postgres=> CREATE DATABASE A ENCODING 'UTF8' template = template0;
      postgres=> CREATE DATABASE B ENCODING 'GBK' template = template0;

#. View the database list. You can view the created databases A and B.

   ::

      postgres=> \l
                                 List of databases
         Name    |  Owner  | Encoding  | Collate | Ctype | Access privileges
      -----------+---------+-----------+---------+-------+-------------------
       a         | dbadmin | UTF8      | C       | C     |
       b         | dbadmin | GBK       | C       | C     |
       gaussdb   | Ruby    | SQL_ASCII | C       | C     |
       postgres  | Ruby    | SQL_ASCII | C       | C     |
       template0 | Ruby    | SQL_ASCII | C       | C     | =c/Ruby          +
                 |         |           |         |       | Ruby=CTc/Ruby
       template1 | Ruby    | SQL_ASCII | C       | C     | =c/Ruby          +
                 |         |           |         |       | Ruby=CTc/Ruby
       xiaodi    | dbadmin | UTF8      | C       | C     |
      (7 rows)

#. Switch to database A and enter the user password. Create a table named **test01** and insert data into the table.

   ::

      postgres=> \c a
      Password for user dbadmin:
      SSL connection (protocol: TLSv1.3, cipher: TLS_AES_128_GCM_SHA256, bits: 128)
      You are now connected to database "a" as user "dbadmin".

      a=> CREATE TABLE test01
       (
           c_customer_sk             integer,
           c_customer_id             char(5),
           c_first_name              char(6),
           c_last_name               char(8)
       )
       with (orientation = column,compression=middle)
       distribute by hash (c_last_name);
      CREATE TABLE
      a=> INSERT INTO test01(c_customer_sk, c_customer_id, c_first_name) VALUES (3769, 'hello', 'Grace');
      INSERT 0 1
      a=> INSERT INTO test01 VALUES (456, 'good');
      INSERT 0 1

#. Run the **\\COPY** command to export data from the UTF8 library in Unicode format to the **test01.dat** file.

   ::

      \copy test01 to '/opt/test01.dat' with (ENCODING 'Unicode');

#. Switch to database B and create a table with the same name **test01**.

   ::

      a=> \c b
      Password for user dbadmin:
      SSL connection (protocol: TLSv1.3, cipher: TLS_AES_128_GCM_SHA256, bits: 128)
      You are now connected to database "b" as user "dbadmin".

      b=> CREATE TABLE test01
       (
           c_customer_sk             integer,
           c_customer_id             char(5),
           c_first_name              char(6),
           c_last_name               char(8)
       )
       with (orientation = column,compression=middle)
       distribute by hash (c_last_name);

#. Run the **\\COPY** command to import the **test01.dat** file to database B.

   ::

      \copy test01 from '/opt/test01.dat' with (ENCODING 'Unicode' ,COMPATIBLE_ILLEGAL_CHARS 'true');

   .. note::

      -  The error tolerance parameter **COMPATIBLE_ILLEGAL_CHARS** specifies that invalid characters are tolerated during data import. Invalid characters are converted and then imported to the database. No error message is displayed. The import is not interrupted.
      -  The BINARY format is not supported. When data of such format is imported, error "cannot specify bulkload compatibility options in BINARY mode" will occur.
      -  The parameter is valid only for data importing using the **COPY FROM** option.

#. View data in the **test01** table in database B.

   ::

      b=> select * from test01;
       c_customer_sk | c_customer_id | c_first_name | c_last_name
      ---------------+---------------+--------------+-------------
                3769 | hello         | Grace        |
                 456 | good          |              |
      (2 rows)

#. After the preceding operations are performed, data is imported from database A (UTF8) to database B (GBK).
