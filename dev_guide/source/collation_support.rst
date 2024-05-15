:original_name: dws_04_0984.html

.. _dws_04_0984:

Collation Support
=================

The collation feature allows specifying the data sorting order and data classification rules in a character set. This alleviates the restriction that the **LC_COLLATE** and **LC_CTYPE** settings of a database cannot be changed after its creation.

Overview
--------

Every expression of a collatable data type has a collation. (The built-in collatable data types are text, varchar, and char. User-defined base types can also be marked collatable, and of course a domain over a collatable data type is collatable.) If the expression is a column reference, the collation of the expression is the defined collation of the column. If the expression is a constant, the collation is the default collation of the data type of the constant. The collation of a more complex expression is derived from the collations of its inputs.

Collation Combination Principles
--------------------------------

-  The collation of an expression can be the default collation, which means the locale settings defined for the database. It is also possible for an expression's collation to be indeterminate. In such cases, ordering operations and other operations that need to know the collation will fail.
-  For a function or operator call, the collation that is derived by examining the argument collations is used at run time for performing the specified operation. If the result of the function or operator call is of a collatable data type, the collation is also used as the defined collation of the function or operator expression, in case there is a surrounding expression that requires knowledge of its collation.
-  The collation derivation of an expression can be implicit or explicit. This distinction affects how collations are combined when multiple different collations appear in an expression. An explicit collation derivation occurs when a **COLLATE** clause is used; all other collation derivations are implicit. When multiple collations need to be combined, the following rules are used:

   -  If any input expression has an explicit collation derivation, then all explicitly derived collations among the input expressions must be the same, otherwise an error is raised. If any explicitly derived collation is present, that is the result of the collation combination.
   -  Otherwise, all input expressions must have the same implicit collation derivation or the default collation. If any non-default collation is present, that is the result of the collation combination. Otherwise, the result is the default collation.
   -  If there are conflicting non-default implicit collations among the input expressions, then the combination is deemed to have indeterminate collation. This is not an error condition unless the particular function being invoked requires knowledge of the collation it should apply. If it does, an error will be raised at run-time.

-  In a CASE expression, the comparison rule is subject to the COLLATE setting in the WHEN clause.
-  Explicit COLLATE derivation takes effect only in the current query (CTE or SUBQUERY). Outside the query, implicit derivation takes effect.

Collation Tips
--------------

-  Do not use multiple collations in the same query statement. Otherwise, exceptional result sets may be generated.
-  Do not use multiple COLLATE clauses to specify a collation.

Case-insensitive Collation Support
----------------------------------

Since cluster 8.1.3, GaussDB(DWS) has added the built-in case_insensitive collation, which is case-insensitive to character types in some actions (such as sorting, comparison, and hash).

Constraints:

-  Supported character types: char, character, nchar, and varchar/character varying/varchar2/nvarchar2/clob/text.
-  The character types **char** and **name** are not supported.
-  The following encoding formats are not supported: PG_EUC_JIS_2004, PG_MULE_INTERNAL, PG_LATIN10 and PG_WIN874.
-  It cannot be specified to **LC_COLLATE** when **CREATE DATABASE** is executed.
-  Regular expressions are not supported.
-  Record comparison of the character type (for example, **record_eq**) is not supported.
-  Time series tables are not supported.
-  Skew optimization is not supported.
-  RoughCheck optimization is not supported.

Examples
--------

The COLLATE clause is specified in the statement.

::

   SELECT 'a' = 'A', 'a' = 'A' COLLATE case_insensitive;
    ?column? | ?column?
   ----------+----------
    f        | t
   (1 row)

Set the column attribute to **case_insensitive** when creating a table.

::

   CREATE TABLE t1 (a text collate case_insensitive);
   NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using round-robin as the distribution mode by default.
   HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
   CREATE TABLE
   \d t1
               Table "public.t1"
    Column | Type |        Modifiers
   --------+------+--------------------------
    a      | text | collate case_insensitive

   INSERT INTO t1 values('a'),('A'),('b'),('B');
   INSERT 0 4

This parameter is specified during table creation and does not need to be specified during query.

::

   SELECT a, a='a' FROM t1;
    a | ?column?
   ---+----------
    A | t
    B | f
    a | t
    b | f
   (4 rows)
   SELECT a, count(1) FROM t1 GROUP BY a;
    a | count
   ---+-------
    a |     2
    B |     2
   (2 rows)

CASE expression, which is subject to the COLLATE setting in the WHEN clause.

::

   SELECT a,case a when 'a' collate case_insensitive then 'case1' when 'b' collate "C" then 'case2' else 'case3' end FROM t1;
    a | case
   ---+-------
    A | case1
    B | case3
    a | case1
    b | case2
   (4 rows)

Implicit derivation across subqueries.

::

   SELECT * FROM (SELECT a collate "C" from t1) WHERE a in ('a','b');
    a
   ---
    a
    b
   (2 rows)
   SELECT * FROM t1,(SELECT a collate "C" from t1) t2 WHERE t1.a=t2.a;
   ERROR:  could not determine which collation to use for string hashing
   HINT:  Use the COLLATE clause to set the collation explicitly.

.. caution::

   -  **collate case_insensitive** is an insensitive sorting, and the result set is uncertain. If sensitive sorting is used after **collate case_insensitive** sorting, the result set may be unstable. Therefore, do not use sensitive sorting and insensitive sorting together in statements.
   -  If **collate case_insensitive** is used to specify character behaviors as case-insensitive, the performance will be affected. If you require high performance, exercise caution when configuring this parameter.
