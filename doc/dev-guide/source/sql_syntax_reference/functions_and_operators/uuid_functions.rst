:original_name: dws_06_0040.html

.. _dws_06_0040:

UUID Functions
==============

UUID functions are used to generate UUID data (see :ref:`UUID Type <dws_06_0019>`).

.. _en-us_topic_0000001764516474__section1896117291214:

uuid_generate_v1()
------------------

Description: Generates a UUID sequence number.

Return type: UUID

Example:

::

   SELECT uuid_generate_v1();
              uuid_generate_v1
   --------------------------------------
    c71ceaca-a175-11e9-a920-797ff7000001
   (1 row)

.. note::

   The **uuid_generate_v1** function generates a UUID based on the time information, cluster node ID, and ID of the thread that generates the sequence. The UUID is globally unique in a single cluster, however, there is a possibility that the time information, cluster node IDs, thread IDs, and clock sequences of multiple clusters are the same at the same time. Therefore, there is a low probability that UUIDs generated among multiple clusters are duplicate.

uuid()
------

Description: Generates a UUID sequence number. This function is a MySQL compatibility function supported only by version 8.2.0 and later clusters.

Return type: UUID

Example:

::

   SELECT uuid();
                uuid
   ----------------------------------
    6327dc96-f0e7-0100-f2f2-6c9ff700fffe
   (1 row)

.. note::

   UUID is generated in the way the :ref:`uuid_generate_v1() <en-us_topic_0000001764516474__section1896117291214>` function works. The function generates a UUID based on the time information, cluster node ID, and ID of the thread that generates the sequence. The UUID is globally unique in a single cluster, however, there is a possibility that the time information, cluster node IDs, thread IDs, and clock sequences of multiple clusters are the same at the same time. Therefore, there is a low probability that UUIDs generated among multiple clusters are duplicate.

sys_guid()
----------

Description: Generates an Oracle GUID, which is similar to the UUID. This function is compatible with Oracle.

Return type: text

Example:

::

   SELECT sys_guid();
                sys_guid
   ----------------------------------
    4EBD3C74A17A11E9A1BF797FF7000001
   (1 row)

.. note::

   The data generation principle of the sys_guid function is the same as that of the uuid_generate_v1 function.

UUID Function Application Example
---------------------------------

A UUID is globally unique. It can be used as a primary key or a distribution column in a data table. When **uuid_generate_v1()** is used as the default value of the distribution column in a data table, data can be evenly distributed to each DN through hash distribution to prevent data skew.

.. note::

   The obvious advantage of UUID is that it is globally unique and does not require a central node. UUIDs are generated independently on a single node. However, UUIDs occupy more storage space than INTs, the index efficiency is low, and the generated IDs are random, making them difficult to identify. Therefore, you need to select UUID or Sequence as the primary key of the data table based on the site requirements.

An example is as follows:

#. The INT type is used as the distribution column.

   An example hash table **mytable01** is created, and the ints are used as the distribution column. After data is inserted, data skew occurs.

   ::

      CREATE TABLE mytable01(a INT, b INT) DISTRIBUTE BY hash(a);
      CREATE TABLE
      INSERT INTO mytable01 VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable01 VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable01 VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable01 VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable01 VALUES(1, 10);
      INSERT 0 1
      SELECT * FROM mytable01;
       a | b
      ---+----
       1 | 10
       1 | 10
       1 | 10
       1 | 10
       1 | 10
      (5 rows)

      SELECT table_skewness('mytable01');
                 table_skewness
      -------------------------------------
       ("dn_6003_6004        ",5,100.000%)
       ("dn_6001_6002        ",0,0.000%)
       ("dn_6005_6006        ",0,0.000%)
      (3 rows)

#. The UUIDs are used as the distribution column.

   Create an example hash table **mytable02** and use the UUIDs as the distribution column. After data is inserted, data distribution is normal.

   ::

      CREATE TABLE mytable02 (id UUID default uuid_generate_v1(), a INT, b INT) DISTRIBUTE BY hash(id);
      CREATE TABLE

      INSERT INTO mytable02(a, b) VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable02(a, b) VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable02(a, b) VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable02(a, b) VALUES(1, 10);
      INSERT 0 1
      INSERT INTO mytable02(a, b) VALUES(1, 10);
      INSERT 0 1

      SELECT * FROM mytable02;
                        id                  | a | b
      --------------------------------------+---+----
       63e45c14-cc74-0e00-e9aa-0a2c3fa0fffe | 1 | 10
       63e45c1f-4d18-0700-e9ab-0a2c3fa0fffe | 1 | 10
       63e45c26-f859-0b00-e9ad-0a2c3fa0fffe | 1 | 10
       63e45c23-9e5d-0300-e9ac-0a2c3fa0fffe | 1 | 10
       63e45c2a-5825-0600-e9ae-0a2c3fa0fffe | 1 | 10
      (5 rows)

      SELECT table_skewness('mytable02');
                 table_skewness
      ------------------------------------
       ("dn_6001_6002        ",3,60.000%)
       ("dn_6003_6004        ",2,40.000%)
       ("dn_6005_6006        ",0,0.000%)
      (3 rows)
