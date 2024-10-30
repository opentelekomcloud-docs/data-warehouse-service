:original_name: dws_06_0080.html

.. _dws_06_0080:

UNION, CASE, and Related Constructs
===================================

SQL UNION constructs must match up possibly dissimilar types to become a single result set. Since all query results from a **SELECT UNION** statement must appear in a single set of columns, the types of the results of each **SELECT** clause must be matched up and converted to a uniform set. Similarly, the result expressions of a **CASE** construct must be converted to a common type so that the **CASE** expression as a whole has a known output type. The same holds for **ARRAY** constructs, and for the **GREATEST** and **LEAST** functions.

Type Resolution for UNION, CASE, and Related Constructs
-------------------------------------------------------

-  If all inputs are of the same type, and it is not unknown, resolve as that type.
-  If all inputs are of type **unknown**, resolve as type **text** (the preferred type of the string category). Otherwise, **unknown** inputs are ignored.
-  If the non-unknown inputs are not all of the same type category, the query fails. (Type **unknown** is not included.)
-  If the non-unknown inputs are all of the same type category, choose the first non-unknown input type which is a preferred type in that category, if there is one. (Exception: The UNION operation regards the type of the first branch as the selected type.)

   .. note::

      **typcategory** in the **pg_type** system catalog indicates the data type category. **typispreferred** indicates whether a type is preferred in **typcategory**.

-  All the input is converted to the selected type. (The original length of a string is retained). Fail if there is not an implicit conversion from a given input to the selected type.
-  If the input contains the json, txid_snapshot, sys_refcursor, or geometry type, **UNION** cannot be performed.

Type Resolution for CASE, COALESCE, IF, and IFNULL in TD-Compatible Mode
------------------------------------------------------------------------

-  If all inputs are of the same type, and it is not unknown, resolve as that type.
-  If all inputs are of type **unknown**, resolve as type **text**.
-  If inputs are of string type (including **unknown** which is resolved as type **text**) and digit type, resolve as the string type. If the inputs are not of the two types, fail.
-  If the non-unknown inputs are all of the same type category, choose the input type which is a preferred type in that category, if there is one.
-  Convert all inputs to the selected type. Fail if there is not an implicit conversion from a given input to the selected type.

Type Resolution for CASE, COALESCE, IF, and IFNULL in MySQL-Compatible Mode
---------------------------------------------------------------------------

-  If all inputs are of the same type, and it is not unknown, resolve as that type.
-  If all inputs are of type **unknown**, resolve as type **text**.
-  If some inputs are of type **unknown** and the others are of a non-**unknown** type, resolve as that non-**unknown** type.
-  If the inputs are of different non-**unknown** types, treat type **enum** as type **text** for comparison.
-  If the non-**unknown** inputs are all of the same type, choose a preferred type, if there is one. If the inputs are of different types, resolve as type **text**.
-  Convert all inputs to the selected type. Fail if there is not an implicit conversion from a given input to the selected type.

Examples
--------

Example 1: Use type resolution with unknown types in a union as the first example. Here, the unknown-type literal **'b'** will be resolved to type **text**.

::

   SELECT text 'a' AS "text" UNION SELECT 'b';
    text
   ------
    a
    b
   (2 rows)

Example 2: Use type resolution in a simple union as the second example. The literal **1.2** is of type **numeric**, and the **integer** value **1** can be cast implicitly to **numeric**, so that type is used.

::

   SELECT 1.2 AS "numeric" UNION SELECT 1;
    numeric
   ---------
          1
        1.2
   (2 rows)

Example 3: Use type resolution in a transposed union as the third example. Here, since type **real** cannot be implicitly cast to **integer**, but **integer** can be implicitly cast to **real**, the union result type is resolved as **real**.

::

   SELECT 1 AS "real" UNION SELECT CAST('2.2' AS REAL);
    real
   ------
       1
     2.2
   (2 rows)

Example 4: Use type resolution in the **COALESCE** function with input values of types **int** and **varchar** as the fourth example. Type resolution fails in ORA-compatible mode. The types are resolved as type **varchar** in TD-compatible mode, and as type **text** in MySQL-compatible mode.

::

   -- Create the ora_db, td_db, and mysql_db databases by setting dbcompatibility to ORA, TD, and MySQL, respectively:
   CREATE DATABASE ora_db dbcompatibility = 'ORA';
   CREATE DATABASE td_db dbcompatibility = 'TD';
   CREATE DATABASE mysql_db dbcompatibility = 'MySQL';

   -- Switch to the ora_db database:
   \c ora_db

   -- Create the t1 table:
   ora_db=# CREATE TABLE t1(a int, b varchar(10));

   -- Show the execution plan of a statement for querying the types int and varchar of input parameters for COALESCE:
   ora_db=# EXPLAIN SELECT coalesce(a, b) FROM t1;
   ERROR:  COALESCE types integer and character varying cannot be matched
   CONTEXT:  referenced column: coalesce

   -- Delete the table:
   ora_db=# DROP TABLE t1;

   -- Switch to the td_db database:
   ora_db=# \c td_db

   -- Create the t2 table:
   td_db=# CREATE TABLE t2(a int, b varchar(10));

   -- Show the execution plan of a statement for querying the types int and varchar of input parameters for COALESCE:
   td_db=# EXPLAIN VERBOSE select coalesce(a, b) from t2;
                                             QUERY PLAN
   -----------------------------------------------------------------------------------------------
     id |                  operation                   | E-rows | E-distinct | E-width | E-costs
    ----+----------------------------------------------+--------+------------+---------+---------
      1 | ->  Data Node Scan on "__REMOTE_FQS_QUERY__" |      0 |            |       0 | 0.00

                          Targetlist Information (identified by plan id)
    -------------------------------------------------------------------------------------------
      1 --Data Node Scan on "__REMOTE_FQS_QUERY__"
            Output: (COALESCE((t2.a)::character varying, t2.b))
            Node/s: All datanodes
            Remote query: SELECT COALESCE(a::character varying, b) AS "coalesce" FROM public.t2
   (10 rows)

   -- Delete the table:
   td_db=# DROP TABLE t2;

   -- Switch to the mysql_db database:
   td_db=# \c mysql_db

   -- Create the t3 table:
   mysql_db=# CREATE TABLE t3(a int, b varchar(10));

   -- Show the execution plan of a statement for querying the types int and varchar of input parameters for COALESCE:
   mysql_db=# EXPLAIN VERBOSE select coalesce(a, b) from t3;
                                             QUERY PLAN
   -----------------------------------------------------------------------------------------------
     id |                  operation                   | E-rows | E-distinct | E-width | E-costs
    ----+----------------------------------------------+--------+------------+---------+---------
      1 | ->  Data Node Scan on "__REMOTE_FQS_QUERY__" |      0 |            |       0 | 0.00

                       Targetlist Information (identified by plan id)
    ------------------------------------------------------------------------------------
      1 --Data Node Scan on "__REMOTE_FQS_QUERY__"
            Output: (COALESCE((t3.a)::text, (t3.b)::text))
            Node/s: All datanodes
            Remote query: SELECT COALESCE(a::text, b::text) AS "coalesce" FROM public.t3
   (10 rows)

   -- Delete the table:
   mysql_db=# DROP TABLE t3;

   -- Switch to the gaussdb database.
   mysql_db=# \c gaussdb

   -- Delete the databases:
   DROP DATABASE ora_db;
   DROP DATABASE td_db;
   DROP DATABASE mysql_db;
