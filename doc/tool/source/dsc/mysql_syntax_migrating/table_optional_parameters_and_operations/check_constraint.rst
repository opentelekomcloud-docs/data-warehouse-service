:original_name: dws_16_0155.html

.. _dws_16_0155:

CHECK Constraint
================

Specifies an expression producing a Boolean result which new or updated rows must satisfy for an insert or update operation to succeed. Expressions evaluating to **TRUE** or **UNKNOWN** succeed. If any row of an insert or update operation produces a FALSE result, an error exception is raised and the insert or update does not alter the database.

A check constraint specified as a column constraint should reference only the column's values, while an expression appearing in a table constraint can reference multiple columns.

CREATE TABLE CHECK
------------------

When creating a table in GaussDB(DWS), you can place the **CHECK** constraint of a column after the column field or below the column field. The syntax is as follows: CHECK (column name > 0), To name the **CHECK** constraint or define the **CHECK** constraint of multiple columns, use the **CONSTRAINT chk_name CHECK (column_namw1 >0 AND column_name2='x>x")** syntax.

.. caution::

   The CHECK constraint is also removed if the table is removed.

**Input**

::

   DROP TABLE IF EXISTS `t1`;
   CREATE TABLE IF NOT EXISTS t1  (
     id int(25) not null primary key check (id > 1 and id < 100),
   city varchar(255) not null unique check (city='city 1' or city='city 2' or city='city 3'),
     population int(25) not null ,
     constraint t1_1 check (population>0 and population<10000)
   ) ;

**Output**

::

   DROP TABLE IF EXISTS "public"."t1";
   CREATE TABLE IF NOT EXISTS "public"."t1" (
     "id" INTEGER NOT NULL PRIMARY KEY CHECK (
       id > 1
       and id < 100
     ),
     "city" VARCHAR(1020) NOT NULL CHECK (
   city ='City 1'
   or city ='City 2'
   or city ='City 3'
     ),
     "population" INTEGER NOT NULL,
     CONSTRAINT t1_1 CHECK (
       population > 0
       and population < 10000
     )
   ) WITH (ORIENTATION = ROW, COMPRESSION = NO) NOCOMPRESS DISTRIBUTE BY HASH ("id");

ALTER TABLE CHECK
-----------------

When you perform operations on table columns, you can have or have no naming constraint. To create a CHECK constraint in a column, use the **ALTER TABLE** *Table name* **ADD CHECK (<**\ *Check constraint*\ **>);** syntax.

To name a **CHECK** constraint and define it for multiple columns, use the **ALTER TABLE** *Table name* **ADD CONSTRAINT <**\ *Name of the check constraint*> **CHECK(<**\ *Check constraint*\ **>);** syntax.

**Input**

::

   ALTER TABLE t1 add check (id>1 and id > 2  and id < 100 or id =50);
   ALTER TABLE student add constraint check02 check (id > 1 and class = '3');
   ALTER TABLE t1 add check (class=1 or class =2  or class = 3 or class=4 or class = 5 or class = 6);
   ALTER TABLE t1 add check (city='city 1' or city='city 2' or city='city 3');

**Output**

::

   ALTER TABLE "public"."t1" ADD CHECK ( id > 1  and id > 2 and id < 100  or id = 50 );
   ALTER TABLE "public"."student" ADD CONSTRAINT check02 CHECK (  id > 1  and class = '3');
   ALTER TABLE "public"."t1" ADD CHECK ( class = 1 or class = 2 or class = 3  or class = 4 or class = 5 or class = 6);
   ALTER TABLE "public"."t1" ADD CHECK (city ='City 1' or city =' City 2' or city =' City 3');
