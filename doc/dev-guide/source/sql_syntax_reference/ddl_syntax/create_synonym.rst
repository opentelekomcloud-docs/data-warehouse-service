:original_name: dws_06_0176.html

.. _dws_06_0176:

CREATE SYNONYM
==============

Function
--------

**CREATE SYNONYM** is used to create a synonym object. A synonym is an alias of a database object and is used to record the mapping between database object names. You can use synonyms to access associated database objects.

Precautions
-----------

-  The user of a synonym should be its owner.
-  If the schema name is specified, create a synonym in the specified schema. Otherwise create a synonym in the current schema.
-  Database objects that can be accessed using synonyms include tables, views, functions, and stored procedures.
-  To use synonyms, you must have the required permissions on associated objects.
-  The following DML statements support synonyms: **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **EXPLAIN**, and **CALL**.
-  The **CREATE SYNONYM** statement of an associated function or stored procedure cannot be used in a stored procedure. You are advised to use synonyms existing in the **pg_synonym** system catalog in the stored procedure.

Syntax
------

::

   CREATE [ OR REPLACE ] SYNONYM synonym_name
       FOR object_name;

Parameter Description
---------------------

-  **synonym**

   Name of a synonym which is created (optionally with schema names)

   Value range: a string. It must comply with the identifier naming rules.

-  **object_name**

   Name of an object that is associated (optionally with schema names)

   Value range: a string. It must comply with the identifier naming rules.

   .. note::

      **object_name** can be the name of an object that does not exist.

Examples
--------

Create schema **ot**:

::

   CREATE SCHEMA ot;

Create table **ot.t1** and its synonym **t1**:

::

   CREATE TABLE ot.t1(id int, name varchar2(10)) DISTRIBUTE BY hash(id);
   CREATE OR REPLACE SYNONYM t1 FOR ot.t1;

Use synonym **t1**:

::

   SELECT * FROM t1;
   INSERT INTO t1 VALUES (1, 'ada'), (2, 'bob');
   UPDATE t1 SET t1.name = 'cici' WHERE t1.id = 2;

Create synonym **v1** and its associated view **ot.v_t1**:

::

   CREATE SYNONYM v1 FOR ot.v_t1;
   CREATE VIEW ot.v_t1 AS SELECT * FROM ot.t1;

Use synonym **v1**:

::

   SELECT * FROM v1;

Create overloaded function **ot.add** and its synonym **add**:

::

   CREATE OR REPLACE FUNCTION ot.add(a integer, b integer) RETURNS integer AS
   $$
   SELECT $1 + $2
   $$
   LANGUAGE sql;

   CREATE OR REPLACE FUNCTION ot.add(a decimal(5,2), b decimal(5,2)) RETURNS decimal(5,2) AS
   $$
   SELECT $1 + $2
   $$
   LANGUAGE sql;

   CREATE OR REPLACE SYNONYM add FOR ot.add;

Use synonym **add**:

::

   SELECT add(1,2);
   SELECT add(1.2,2.3);

Create stored procedure **ot.register** and its synonym **register**:

::

   CREATE PROCEDURE ot.register(n_id integer, n_name varchar2(10))
   SECURITY INVOKER
   AS
   BEGIN
       INSERT INTO ot.t1 VALUES(n_id, n_name);
   END;
   /

   CREATE OR REPLACE SYNONYM register FOR ot.register;

Use synonym **register** to invoke the stored procedure:

::

   CALL register(3,'mia');

Helpful Links
-------------

:ref:`ALTER SYNONYM <dws_06_0140>` :ref:`DROP SYNONYM <dws_06_0207>`
