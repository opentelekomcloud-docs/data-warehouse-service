:original_name: dws_gsql_003.html

.. _dws_gsql_003:

Instruction
===========

Downloading and Installing gsql and Using It to Connect to the Cluster Database
-------------------------------------------------------------------------------

For details about how to download and install gsql and connect it to the cluster database, see section "Using the gsql CLI Client to Connect to a Cluster" in the *Data Warehouse Service (DWS) User Guide*.

Example
-------

The example shows how to spread a command over several lines of input. Pay attention to prompt changes:

::

   postgres=# CREATE TABLE HR.areaS(
   postgres(# area_ID   NUMBER,
   postgres(# area_NAME VARCHAR2(25)
   postgres-# )tablespace EXAMPLE;
   CREATE TABLE

View the table definition.

::

   \d HR.areaS
                  Table "hr.areas"
     Column   |         Type          | Modifiers
   -----------+-----------------------+-----------
    area_id   | numeric               | not null
    area_name | character varying(25) |

Insert four lines of data into **HR.areaS**.

::

   INSERT INTO HR.areaS (area_ID, area_NAME) VALUES (1, 'Wood');
   INSERT 0 1
   INSERT INTO HR.areaS (area_ID, area_NAME) VALUES (2, 'Lake');
   INSERT 0 1
   INSERT INTO HR.areaS (area_ID, area_NAME) VALUES (3, 'Desert');
   INSERT 0 1
   INSERT INTO HR.areaS (area_ID, area_NAME) VALUES (4, 'Iron');
   INSERT 0 1

Change the prompt.

::

   \set PROMPT1 '%n@%m %~%R%#'
   dbadmin@[local] postgres=#

View the table.

::

   dbadmin@[local] postgres=#SELECT * FROM HR.areaS;
    area_id |       area_name
   ---------+------------------------
          1 | Wood
          4 | Iron
          2 | Lake
          3 | Desert
   (4 rows)

Run the **\\pset** command to display the table in different ways.

::

   dbadmin@[local] postgres=#\pset border 2
   Border style is 2.
   dbadmin@[local] postgres=#SELECT * FROM HR.areaS;
   +---------+------------------------+
   | area_id |       area_name        |
   +---------+------------------------+
   |       1 | Wood                 |
   |       2 | Lake               |
   |       3 | Desert                   |
   |       4 | Iron |
   +---------+------------------------+
   (4 rows)

::

   dbadmin@[local] postgres=#\pset border 0
   Border style is 0.
   dbadmin@[local] postgres=#SELECT * FROM HR.areaS;
   area_id       area_name
   ------- ----------------------
         1 Wood
         2 Lake
         3 Desert
         4 Iron
   (4 rows)

Use the meta-command.

::

   dbadmin@[local] postgres=#\a \t \x
   Output format is unaligned.
   Showing only tuples.
   Expanded display is on.
   dbadmin@[local] postgres=#SELECT * FROM HR.areaS;
   area_id|2
   area_name|Lake

   area_id|1
   area_name|Wood

   area_id|4
   area_name|Iron

   area_id|3
   area_name|Desert
   dbadmin@[local] postgres=#
