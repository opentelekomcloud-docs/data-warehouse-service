:original_name: dws_06_0238.html

.. _dws_06_0238:

SELECT
======

Function
--------

Retrieves data from a table or view.

Serving as an overlaid filter for a database table, **SELECT** using SQL keywords retrieves required data from data tables.

Precautions
-----------

-  Using **SELECT** can join HDFS and ordinary tables, but cannot join ordinary and GDS foreign tables. That is, a **SELECT** statement cannot contain both ordinary and GDS foreign tables.
-  The user must have the SELECT permission on every column used in the **SELECT** command.

-  UPDATE permission is required when using **FOR UPDATE** or **FOR SHARE**.

.. warning::

   -  Avoid SQL statements that cannot be pushed down (indicated by **REMOTE_XXX_QUERY** in the **EXPLAIN** output).
   -  Refrain from frequent **COUNT** queries on large row-store tables (over 10 million rows).
   -  Limit the number of **UNION**/**UNION ALL** branches in a single SQL statement to 50, and associate no more than 25 non-replicated tables.
   -  Prevent Cartesian products by ensuring proper association conditions during large table joins.
   -  In multi-table association queries, keep the number of streams in a single statement below 100.
   -  Use the **WITH RECURSIVE** statement cautiously. Define data repetition limits and termination conditions to ensure recursion ends as expected.
   -  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

-  Querying data

::

   [ WITH [ RECURSIVE ] with_query [, ...] ]
   SELECT [/*+ plan_hint */] [ ALL | DISTINCT [ ON ( expression [, ...] ) ] ]
   { * | {expression [ [ AS ] output_name ]} [, ...] }
   [ FROM from_item [, ...] ]
   [ WHERE condition ]
   [ GROUP BY grouping_element [, ...] ]
   [ HAVING condition [, ...] ]
   [ WINDOW {window_name AS ( window_definition )} [, ...] ]
   [ { UNION | INTERSECT | EXCEPT | MINUS } [ ALL | DISTINCT ] select ]
   [ ORDER BY {expression [ [ ASC | DESC | USING operator ] | nlssort_expression_clause ] [ NULLS { FIRST | LAST } ]} [, ...] ]
   [ { [ LIMIT { count | ALL } ] [ OFFSET start [ ROW | ROWS ] ] } | { LIMIT start, { count | ALL } } ]
   [ FETCH { FIRST | NEXT } [ count ] { ROW | ROWS } ONLY ]
   [ {FOR { UPDATE | SHARE } [ OF table_name [, ...] ] [ NOWAIT ]} [...] ];

.. note::

   In **condition** and **expression**, you can use the aliases of expressions in the output column in compliance with the following rules:

   -  Reference only in the same level.
   -  Only reference aliases in the output column.
   -  Reference a prior expression in a subsequent expression.
   -  The **volatile** function cannot be used.
   -  The **Window** function cannot be used.
   -  Do not reference an alias in the **join on** condition.
   -  An error is reported if the output column contains multiple referenced aliases.

-  The subquery **with_query** is as follows:

   ::

      with_query_name [ ( column_name [, ...] ) ]
          AS [ [ NOT ] MATERIALIZED ] ( {select | values | insert | update | delete} )

-  The specified query source **from_item** is as follows:

   ::

      {[ ONLY ] table_name [ * ] [ partition_clause ] [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
      |( select ) [ AS ] alias [ ( column_alias [, ...] ) ]
      |with_query_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
      |function_name ( [ argument [, ...] ] ) [ AS ] alias [ ( column_alias [, ...] | column_definition [, ...] ) ]
      |function_name ( [ argument [, ...] ] ) AS ( column_definition [, ...] )
      |from_item [ NATURAL ] join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]}

-  The **group** clause is as follows:

   ::

      ( )
      | expression
      | ( expression [, ...] )
      | ROLLUP ( { expression | ( expression [, ...] ) } [, ...] )
      | CUBE ( { expression | ( expression [, ...] ) } [, ...] )
      | GROUPING SETS ( grouping_element [, ...] )

-  The specified partition **partition_clause** is as follows:

   ::

      PARTITION { ( partition_name ) |
              FOR (  partition_value [, ...] ) }

   .. note::

      Partitions can be specified only for ordinary tables.

-  The sorting order **nlssort_expression_clause** is as follows:

   ::

      NLSSORT ( column_name, ' NLS_SORT = { SCHINESE_PINYIN_M | generic_m_ci } ' )

-  Simplified query syntax, equivalent to **select \* from table_name**.

   ::

      TABLE { ONLY {(table_name)| table_name} | table_name [ * ]};

.. _en-us_topic_0000001811515533__s3d562432879c4244bcdbfdf9f30bcc5e:

Parameter Description
---------------------

-  **WITH [ RECURSIVE ] with_query [, ...]**

   The **WITH** clause allows you to specify one or more subqueries that can be referenced by name in the primary query, equal to temporary table.

   If **RECURSIVE** is specified, it allows a **SELECT** subquery to reference itself by name.

   The syntax of **with_query** is: **with_query_name [ ( column_name [, ...] ) ] AS [ [ NOT ] MATERIALIZED ] ( {select \| values \| insert \| update \| delete} )**

   -  **with_query_name** specifies the name of the result set generated by a subquery. Such names can be used to access the result sets of subqueries in a query.
   -  By default, the **with_query** that is referenced multiple times by the primary query is executed only once. Its result set is materialized so that the primary query can query its result set multiple times. The **with_query** referenced once by the primary query will not be executed independently. Instead, its subquery takes the place where the primary query can directly reference it and is executed with the primary query. If **[NOT] MATERIALIZED** is specified, the default action is changed.

      -  If **MATERIALIZED** is specified, the subquery is executed once and its result set is materialized.
      -  If **NOT MATERIALIZED** is specified, its subquery takes the place where the primary query can directly reference it. **NOT MATERIALIZED** is ignored in the following cases:

         -  The subquery contains volatile functions.

         -  The subquery is a **SELECT** or **VALUES** statement containing **FOR UPDATE** or **FOR SHARE**.

         -  The subquery is an **INSERT**, **UPDATE**, or **DELETE** statement.

         -  **RECURSIVE** is specified for **with_query**.

         -  If **with_query2** is referenced more than once and it references **with_query1**, which referenced itself in the outer layer, **with_query2** cannot take the place where it can be referenced.

            For example, in the following example, **tmp2** is referenced twice. Because **tmp2** references **tmp1** which referenced itself in the outer layer, **tmp2** will be materialized even if **NOT MATERIALIZED** is specified.

            ::

               with recursive tmp1(b) as (values(1)
               union all
               (with tmp2 as not materialized (select * from tmp1)
                select tt1.b + tt2.b from tmp2 tt1, tmp2 tt2))
                select * from tmp1;

   -  **column_name** specifies a column name displayed in the subquery result set.
   -  Each subquery can be a **SELECT**, **VALUES**, **INSERT**, **UPDATE** or **DELETE** statement.

-  **plan_hint** clause

   Follows the **SELECT** keyword in the /``*+``\ <*Plan hint*> \*/ format. It is used to optimize the plan of a **SELECT** statement block. For details, see section "Hint-based Tuning."

-  **ALL**

   Specifies that all rows meeting the requirements are returned. This is the default behavior, so you can omit this keyword.

-  **DISTINCT [ ON ( expression [, ...] ) ]**

   Removes all duplicate rows from the **SELECT** result so one row is kept from each group of duplicates.

   **ON ( expression [, ...] )** is only reserved for the first row among all the rows with the same result calculated using given expressions.

   .. important::

      **DISTINCT ON** expression is explained with the same rule of **ORDER BY**. Unless you use **ORDER BY** to guarantee that the required row appears first, you cannot know what the first row is.

-  **SELECT list**

   Indicates columns to be queried. Some or all columns (using wildcard character \*) can be queried.

   You may use the **AS output_name** clause to give an alias for an output column. The alias is used for the displaying of the output column.

   Column names may be either of:

   -  Manually input column names which are spaced using commas (,).
   -  Fields computed in the **FROM** clause.

-  **FROM** clause

   Indicates one or more source tables for **SELECT**.

   The **FROM** clause can contain the following elements:

   -  table_name

      Indicates the name (optionally schema-qualified) of an existing table or view, for example, **schema_name.table_name**.

   -  alias

      Gives a temporary alias to a table to facilitate the quotation by other queries.

      An alias is used for brevity or to eliminate ambiguity for self-joins. When an alias is provided, it completely hides the actual name of the table or function.

   -  column_alias

      Specifies the column alias.

   -  PARTITION

      Queries data in the specified partition in a partition table.

   -  partition_name

      Specifies the name of a partition.

   -  partition_value

      Specifies the value of the specified partition key. If there are many partition keys, use the **PARTITION FOR** clause to specify the value of the only partition key you want to use.

   -  subquery

      Performs a subquery in the **FROM** clause. A temporary table is created to save subquery results.

   -  with_query_name

      **WITH** clause can also be the source of **FROM** clause and can be referenced with the name queried by executing **WITH**.

   -  function_name

      Function name. Function calls can appear in the **FROM** clause.

   -  join_type

      There are five types below:

      -  [ INNER ] JOIN

         A **JOIN** clause combines two **FROM** items. Use parentheses if necessary to determine the order of nesting. In the absence of parentheses, **JOIN** nests left-to-right.

         In any case, **JOIN** binds more tightly than the commas separating **FROM** items.

      -  LEFT [ OUTER ] JOIN

         Returns all rows in the qualified Cartesian product (all combined rows that pass its join condition), and pluses one copy of each row in the left-hand table for which there was no right-hand row that passed the join condition. This left-hand row is extended to the full width of the joined table by inserting **NULL** values for the right-hand columns. Note that only the **JOIN** clause's own condition is considered while deciding which rows have matches. Outer conditions are applied afterwards.

      -  RIGHT [ OUTER ] JOIN

         Returns all the joined rows, plus one row for each unmatched right-hand row (extended with **NULL** on the left).

         This is just a notational convenience, since you could convert it to a **LEFT OUTER JOIN** by switching the left and right inputs.

      -  FULL [ OUTER ] JOIN

         Returns all the joined rows, pluses one row for each unmatched left-hand row (extended with **NULL** on the right), and pluses one row for each unmatched right-hand row (extended with **NULL** on the left).

      -  CROSS JOIN

         **CROSS JOIN** is equivalent to **INNER JOIN ON (TRUE)**, which means no rows are removed by qualification. These join types are just a notational convenience, since they do nothing you could not do with plain **FROM** and **WHERE**.

         .. note::

            For the **INNER** and **OUTER** join types, a join condition must be specified, namely exactly one of **NATURAL ON**, **join_condition**, or **USING (join_column [, ...])**. For **CROSS JOIN**, none of these clauses can appear.

      **CROSS JOIN** and **INNER JOIN** produce a simple Cartesian product, the same result as you get from listing the two items at the top level of **FROM**.

   -  ON join_condition

      A join condition to define which rows have matches in joins. Example: ON left_table.a = right_table.a

   -  USING(join_column[, ...])

      ON left_table.a = right_table.a AND left_table.b = right_table.b ... abbreviation. Corresponding columns must have the same name.

   -  NATURAL

      **NATURAL** is a shorthand for a **USING** list that mentions all columns in the two tables that have the same names.

   -  from item

      Specifies the name of the query source object connected.

-  **WHERE clause**

   The **WHERE** clause forms an expression for row selection to narrow down the query range of **SELECT**. The condition is any expression that evaluates to a result of Boolean type. Rows that do not satisfy this condition will be eliminated from the output.

   In the **WHERE** clause, you can use the operator (+) to convert a table join to an outer join. However, this method is not recommended because it is not the standard SQL syntax and may raise syntax compatibility issues during platform migration. There are many restrictions on using the operator (+):

   #. It can appear only in the **WHERE** clause.
   #. If a table join has been specified in the **FROM** clause, the operator (+) cannot be used in the **WHERE** clause.
   #. The operator (+) can work only on columns of tables or views, instead of on expressions.
   #. If table A and table B have multiple join conditions, the operator (+) must be specified in all the conditions. Otherwise, the operator (+) will not take effect, and the table join will be converted into an inner join without any prompt information.
   #. Tables specified in a join condition where the operator (+) works cannot cross queries or subqueries. If tables where the operator (+) works are not in the **FROM** clause of the current query or subquery, an error will be reported. If a peer table for the operator (+) does not exist, no error will be reported and the table join will be converted into an inner join.
   #. Expressions where the operator (+) is used cannot be directly connected through **OR**.
   #. If a column where the operator (+) works is compared with a constant, the expression becomes a part of the join condition.
   #. A table cannot have multiple foreign tables.
   #. The operator (+) can appear only in the following expressions: comparison, NOT, ANY, ALL, IN, NULLIF, IS DISTINCT FROM, and IS OF expressions. It is not allowed in other types of expressions. In addition, these expressions cannot be connected through **AND** or **OR**.
   #. The operator (+) can be used to convert a table join only to a left or right outer join, instead of a full join. That is, the operator (+) cannot be specified on both tables of an expression.

   .. important::

      For the **WHERE** clause, if a special character % \_ or \\ is queried in **LIKE**, add the slash (\\) before each character.

   Examples:

   ::

      CREATE TABLE tt01 (id int,content varchar(50));

      INSERT INTO tt01 values (1,'Jack say ''hello''');
      INSERT INTO tt01 values (2,'Rose do 50%');
      INSERT INTO tt01 values (3,'Lilei say ''world''');
      INSERT INTO tt01 values (4,'Hanmei do 100%');

      SELECT * FROM tt01;
       id |      content
      ----+-------------------
        3 | Lilei say 'world'
        4 | Hanmei do 100%
        1 | Jack say 'hello'
        2 | Rose do 50%
      (4 rows)

      SELECT * FROM tt01 WHERE content like '%''he%';
       id |     content
      ----+------------------
        1 | Jack say 'hello'
      (1 row)

      SELECT * FROM tt01 WHERE content like '%50\%%';
       id |   content
      ----+-------------
        2 | Rose do 50%
      (1 row)

-  **GROUP BY clause**

   Condenses query results into a single row or selected rows that share the same values for the grouped expressions.

   -  ROLLUP ( { expression \| ( expression [, ...] ) } [, ...] )

      ROLLUP calculates the standard aggregation value specified by an ordered grouping column in GROUP BY, creates a high-level partial sum from right to left, and finally creates a cumulative sum. A group can be regarded as a series of grouping sets. Example:

      ::

         GROUP BY ROLLUP (a,b,c)

      The preceding condition expression is equivalent to:

      ::

         GROUP BY GROUPING SETS((a,b,c), (a,b), (a), ( ))

      The elements in the **ROLLUP** clause can be independent fields or expressions, or a list contained in parentheses. If it is a list in parentheses, they must be a whole when the grouping set is generated. Example:

      ::

         GROUP BY ROLLUP ((a,b), (c,d))

      The preceding condition expression is equivalent to:

      ::

         GROUPING SETS ((a,b,c,d), (a,b), (c,d ), ( ))

   -  CUBE ( { expression \| ( expression [, ...] ) } [, ...] )

      A CUBE grouping is an extension to the GROUP BY clause that creates subtotals for all of the possible combinations of the given list of grouping columns (or expressions). In terms of multidimensional analysis, CUBE generates all the subtotals that could be calculated for a data cube with the specified dimensions. For example, given three expressions (n=3) in the CUBE clause, the operation results in 2\ :sup:`n` = 2\ :sup:`3` = 8 groupings. Rows grouped on the values of *n* expressions are called regular rows, and the rest are called superaggregate rows. Example:

      ::

         GROUP BY CUBE (a,b,c)

      The preceding condition expression is equivalent to:

      ::

         GROUP BY GROUPING SETS((a,b,c), (a,b), (a,c), (b,c), (a), (b), (c), ( ))

      The elements in the **CUBE** clause can be independent fields or expressions, or a list contained in parentheses. If it is a list in parentheses, they must be a whole when the grouping set is generated. Example:

      ::

         GROUP BY CUBE (a, (b, c), d)

      The preceding condition expression is equivalent to:

      .. code-block::

         GROUP BY GROUPING SETS ((a,b,c,d), (a,b,c), (a), ( ))

   -  GROUPING SETS ( grouping_element [, ...] )

      **GROUPING SETS** is another extension to the **GROUP BY** clause. It allows you to specify multiple **GROUP BY** clauses. The option is used to define a grouping set. Each grouping set needs to be included in a separate parenthesis. A blank parenthesis (()) indicates that all data is processed as a group. This improves efficiency by trimming away unnecessary data. You can specify the required data group for query.

   .. important::

      If the **SELECT** list expression quotes some ungrouped fields and no aggregate function is used, an error is displayed. This is because multiple values may be returned for ungrouped fields.

-  **HAVING clause**

   Selects special groups by working with the **GROUP BY** clause. The **HAVING** clause compares some attributes of groups with a constant. Only groups that matching the logical expression in the **HAVING** clause are extracted.

-  **WINDOW clause**

   The general format is **WINDOW window_name AS ( window_definition ) [, ...]**. **window_name** is a name can be referenced by **window_definition**. **window_definition** can be expressed in the following forms:

   [ existing_window_name ]

   [ PARTITION BY expression [, ...] ]

   [ ORDER BY expression [ ASC \| DESC \| USING operator ] [ NULLS { FIRST \| LAST } ] [, ...] ]

   [ frame_clause ]

   **frame_clause** defines a **window frame** for the window function. The window function (not all window functions) depends on **window frame** and **window frame** is a set of relevant rows of the current query row. **frame_clause** can be expressed in the following forms:

   [ RANGE \| ROWS ] frame_start

   [ RANGE \| ROWS ] BETWEEN frame_start AND frame_end

   **frame_start** and **frame_end** can be expressed in the following forms:

   UNBOUNDED PRECEDING

   value PRECEDING (not supported for **RANGE**)

   CURRENT ROW

   value FOLLOWING (not supported for **RANGE**)

   UNBOUNDED FOLLOWING

   .. important::

      For the query of column storage table, only **row_number** window function is supported, **frame_clause** is not supported.

-  **UNION clause**

   Computes the set union of the rows returned by the involved **SELECT** statements.

   The **UNION** clause has the following constraints:

   -  By default, the result of **UNION** does not contain any duplicate rows unless the **ALL** option is specified.
   -  Multiple **UNION** operators in the same **SELECT** statement are evaluated left to right, unless otherwise specified by parentheses.
   -  **FOR UPDATE** cannot be specified either for a **UNION** result or for any input of a **UNION**.

   General expression:

   select_statement UNION [ALL] select_statement

   -  **select_statement** can be any **SELECT** statement without an **ORDER BY**, **LIMIT**, or **FOR UPDATE** clause.
   -  **ORDER BY** and **LIMIT** in parentheses can be attached in a sub-expression.

-  **INTERSECT clause**

   Computes the set intersection of rows returned by the involved **SELECT** statements. The result of **INTERSECT** does not contain any duplicate rows.

   The **INTERSECT** clause has the following constraints:

   -  Multiple **INTERSECT** operators in the same **SELECT** statement are evaluated left to right, unless otherwise specified by parentheses.
   -  Processing **INTERSECT** preferentially when **UNION** and **INTERSECT** operations are executed for results of multiple **SELECT** statements.

   General format:

   select_statement INTERSECT select_statement

   **select_statement** can be any **SELECT** statement without a **FOR UPDATE** clause.

-  **EXCEPT clause**

   **EXCEPT** clause has the following common form:

   select_statement EXCEPT [ ALL ] select_statement

   **select_statement** can be any **SELECT** statement without a **FOR UPDATE** clause.

   The **EXCEPT** operator computes the set of rows that are in the result of the left **SELECT** statement but not in the result of the right one.

   The result of **EXCEPT** does not contain any duplicate rows unless the **ALL** option is specified. To execute **ALL**, a row that has *m* duplicates in the left table and *n* duplicates in the right table will appear MAX(*m*\ ``-``\ *n*, 0) times in the result set.

   Multiple **EXCEPT** operators in the same **SELECT** statement are evaluated left to right, unless parentheses dictate otherwise. **EXCEPT** binds at the same level as **UNION**.

   Currently, **FOR UPDATE** cannot be specified either for an **EXCEPT** result or for any input of an **EXCEPT**.

-  **MINUS clause**

   Has the same function and syntax as **EXCEPT** clause.

-  **ORDER BY** clause

   Sorts data retrieved by **SELECT** in descending or ascending order. If the **ORDER BY** expression contains multiple columns:

   -  If two columns are equal according to the leftmost expression, they are compared according to the next expression and so on.
   -  If they are equal according to all specified expressions, they are returned in an implementation-dependent order.
   -  Columns sorted by **ORDER BY** must be contained in the result retrieved by **SELECT**.

   .. important::

      -  If **ORDER BY** is not specified, the query results are returned following the generation sequence in the database system.

      -  You can add the keyword **ASC** (in ascending order) or **DESC** (in descending order) next to any expression in the **ORDER BY** clause. If the keyword is not specified, **ASC** is used by default.

      -  To sort query results by case-insensitive Chinese pinyin, set the encoding mode to **UTF-8** or **GBK** during database initialization. The commands are as follows:

         **initdb -E UTF8 -D ../data -locale=zh_CN.UTF-8** or **initdb -E GBK -D ../data -locale=zh_CN.GBK**

-  **[ { [ LIMIT { count \| ALL } ] [ OFFSET start [ ROW \| ROWS ] ] } \| { LIMIT start, { count \| ALL } } ]**

   The **LIMIT** clause consists of two independent **LIMIT** clauses, an **OFFSET** clause, and a **LIMIT** clause with multiple parameters.

   LIMIT { count \| ALL }

   OFFSET start [ ROW \| ROWS ]

   LIMIT start, { count \| ALL }

   **count** in the clauses specifies the maximum number of rows to return, while **start** specifies the number of rows to skip before starting to return rows. When both are specified, **start** rows are skipped before starting to count the **count** rows to be returned. A multi-parameter **LIMIT** clause cannot be used together with a single-parameter **LIMIT** or **OFFSET** clause.

-  **FETCH { FIRST \| NEXT } [ count ] { ROW \| ROWS } ONLY**

   If **count** is omitted in a **FETCH** clause, it defaults to **1**.

-  **FOR UPDATE** clause

   Locks rows retrieved by **SELECT**. This ensures that the rows cannot be modified or deleted by other transactions until the current transaction ends. That is, other transactions that attempt **UPDATE**, **DELETE**, or **SELECT FOR UPDATE** of these rows will be blocked until the current transaction ends.

   To avoid waiting for the committing of other transactions, you can apply **NOWAIT**. Rows to which **NOWAIT** applies cannot be immediately locked. After **SELECT FOR UPDATE NOWAIT** is executed, an error is reported.

   **FOR SHARE** behaves similarly, except that it acquires a shared rather than exclusive lock on each retrieved row. A share lock blocks other transaction from performing **UPDATE**, **DELETE**, or **SELECT FOR UPDATE** on these rows, but it does not prevent them from performing **SELECT FOR SHARE**.

   If specified tables are named in **FOR UPDATE** or FOR SHARE, then only rows coming from those tables are locked; any other tables used in **SELECT** are simply read as usual. Otherwise, locking all tables in the command.

   If **FOR UPDATE** or FOR SHARE is applied to a view or sub-query, it affects all tables used in the view or sub-query.

   Multiple **FOR UPDATE** and **FOR SHARE** clauses can be written if it is necessary to specify different locking behaviors for different tables.

   If the same table is mentioned (or implicitly affected) by both **FOR UPDATE** and **FOR SHARE** clauses, it is processed as **FOR UPDATE**. Similarly, a table is processed as **NOWAIT** if that is specified in any of the clauses affecting it.

   .. important::

      -  For SQL statements containing **FOR UPDATE** or **FOR SHARE**, their execution plans will be pushed down to DNs. If the pushdown fails, an error will be reported.
      -  The query of column storage table does not support **for update/share**.

-  **NLS_SORT**

   Indicates a field to be ordered in a special mode. Currently, only the Chinese Pinyin order and case insensitive order are supported.

   Valid value:

   -  **SCHINESE_PINYIN_M**: Chinese characters are sorted by pinyin. Currently, only level-1 Chinese characters in the GBK character set can be sorted. To use this sort method, specify **GBK** as the encoding format when you create the database. If you do not do so, this value is invalid.
   -  **generic_m_ci**, case-insensitive order.

-  **PARTITION clause**

   Queries data in the specified partition of a partitioned table.

Examples
--------

Obtain the **temp_t** temporary table by a subquery and query all records in this table.

::

   WITH temp_t(name,isdba) AS (SELECT usename,usesuper FROM pg_user) SELECT * FROM temp_t;

Explicitly specify **MATERIALIZED** for the **with_query** named **temp_t**, and then query all data in the **temp_t** table.

::

   WITH temp_t(name,isdba) AS MATERIALIZED (SELECT usename,usesuper FROM pg_user) SELECT * FROM temp_t;

Explicitly specify **NOT MATERIALIZED** for the **with_query** named **temp_t**, and then query all data in the **temp_t** table.

::

   WITH temp_t(name,isdba) AS NOT MATERIALIZED (SELECT usename,usesuper FROM pg_user)
    SELECT * FROM temp_t t1 WHERE name LIKE 'A%'
    UNION ALL
    SELECT * FROM temp_t t2 WHERE name LIKE 'B%';

Query all the **r_reason_sk** records in the **tpcds.reason** table and de-duplicate them.

::

   SELECT DISTINCT(r_reason_sk) FROM tpcds.reason;

Example of a **LIMIT** clause: Obtain a record from the table.

::

   SELECT * FROM tpcds.reason LIMIT 1;

Example of a **LIMIT** clause: Obtain the third record from the table.

::

   SELECT * FROM tpcds.reason LIMIT 1 OFFSET 2;

Example of a **LIMIT** clause: Obtain the first two records from a table.

::

   SELECT * FROM tpcds.reason LIMIT 2;

Query all records and sort them in alphabetic order.

::

   SELECT r_reason_desc FROM tpcds.reason ORDER BY r_reason_desc;

Use table aliases to obtain data from the **pg_user** and **pg_user_status** tables.

::

   SELECT a.usename,b.locktime FROM pg_user a,pg_user_status b WHERE a.usesysid=b.roloid;

Example of the **FULL JOIN** clause: Join data in the **pg_user** and **pg_user_status** tables.

::

   SELECT a.usename,b.locktime,a.usesuper FROM pg_user a FULL JOIN pg_user_status b on a.usesysid=b.roloid;

Example of the **GROUP BY** clause: Filter data based on query conditions, and group the results.

::

   SELECT r_reason_id, AVG(r_reason_sk) FROM tpcds.reason GROUP BY r_reason_id HAVING AVG(r_reason_sk) > 25;

Example of the **GROUP BY** clause: Group the results by alias.

::

   SELECT r_reason_id AS id FROM tpcds.reason GROUP BY id;

Example of the **GROUP BY CUBE** clause: Filter data based on query conditions, and group the results.

::

   SELECT r_reason_id,AVG(r_reason_sk) FROM tpcds.reason GROUP BY CUBE(r_reason_id,r_reason_sk);

Example of the **GROUP BY GROUPING SETS** clause: Filter data based on query conditions, and group the results.

::

   SELECT r_reason_id,AVG(r_reason_sk) FROM tpcds.reason GROUP BY GROUPING SETS((r_reason_id,r_reason_sk),r_reason_sk);

Example of the **UNION** clause: Merge the names started with W and N in the **r_reason_desc** column in the **tpcds.reason** table.

::

   SELECT r_reason_sk, tpcds.reason.r_reason_desc
       FROM tpcds.reason
       WHERE tpcds.reason.r_reason_desc LIKE 'W%'
   UNION
   SELECT r_reason_sk, tpcds.reason.r_reason_desc
       FROM tpcds.reason
       WHERE tpcds.reason.r_reason_desc LIKE 'N%';

Case-insensitive order:

::

   CREATE TABLE stu_icase_info (id bigint, name text) DISTRIBUTE BY REPLICATION;
   INSERT INTO stu_icase_info VALUES (1, 'aaaa'),(2, 'AAAA');
   SELECT * FROM stu_icase_info ORDER BY NLSSORT (name, 'NLS_SORT = generic_m_ci');
    id | name
   ----+------
     1 | aaaa
     2 | AAAA
   (2 rows)

Create partitioned table **tpcds.reason_p**, insert data, and obtain data from the **P_05_BEFORE** partition of the table.

::

   CREATE TABLE tpcds.reason_p
   (
     r_reason_sk integer,
     r_reason_id character(16),
     r_reason_desc character(100)
   )
   PARTITION BY RANGE (r_reason_sk)
   (
     partition P_05_BEFORE values less than (05),
     partition P_15 values less than (15),
     partition P_25 values less than (25),
     partition P_35 values less than (35),
     partition P_45_AFTER values less than (MAXVALUE)
   );

   INSERT INTO tpcds.reason_p values(3,'AAAAAAAABAAAAAAA','reason 1'),(10,'AAAAAAAABAAAAAAA','reason 2'),(4,'AAAAAAAABAAAAAAA','reason 3'),(10,'AAAAAAAABAAAAAAA','reason 4'),(10,'AAAAAAAABAAAAAAA','reason 5'),(20,'AAAAAAAACAAAAAAA','reason 6'),(30,'AAAAAAAACAAAAAAA','reason 7');

   SELECT * FROM tpcds.reason_p PARTITION (P_05_BEFORE);
    r_reason_sk |   r_reason_id    |   r_reason_desc
   -------------+------------------+------------------------------------
              4 | AAAAAAAABAAAAAAA | reason 3
              3 | AAAAAAAABAAAAAAA | reason 1
   (2 rows)
   ——Query the number of rows in partition P_15:
   SELECT count(*) FROM tpcds.reason_p PARTITION (P_15);
    count
   --------
        3
   (1 row)

Example of the **GROUP BY** clause: Group records in the **tpcds.reason_p** table by **r_reason_id**, and count the number of records in each group.

::

   SELECT COUNT(*),r_reason_id FROM tpcds.reason_p GROUP BY r_reason_id;
    count |   r_reason_id
   -------+------------------
        2 | AAAAAAAACAAAAAAA
        5 | AAAAAAAABAAAAAAA
   (2 rows)

Example of the **GROUP BY CUBE** clause: Filter data based on query conditions, and group the results.

::

   SELECT * FROM tpcds.reason GROUP BY CUBE (r_reason_id,r_reason_sk,r_reason_desc);

Example of the **GROUP BY GROUPING SETS** clause: Filter data based on query conditions, and group the results.

::

   SELECT * FROM tpcds.reason GROUP BY GROUPING SETS ((r_reason_id,r_reason_sk),r_reason_desc);

Example of the **HAVING** clause: Group records in the **tpcds.reason_p** table by **r_reason_id**, count the number of records in each group, and display only values whose number of **r_reason_id** is greater than **2**.

::

   SELECT COUNT(*) c,r_reason_id FROM tpcds.reason_p GROUP BY r_reason_id HAVING c>2;
    c |   r_reason_id
   ---+------------------
    5 | AAAAAAAABAAAAAAA
   (1 row)

Example of the **IN** clause: Group records in the **tpcds.reason_p** table by **r_reason_id**, count the number of records in each group, and display only the numbers of records whose **r_reason_id** is **AAAAAAAABAAAAAAA** or **AAAAAAAADAAAAAAA**.

::

   SELECT COUNT(*),r_reason_id FROM tpcds.reason_p GROUP BY r_reason_id HAVING r_reason_id IN('AAAAAAAABAAAAAAA','AAAAAAAADAAAAAAA');
   count |   r_reason_id
   -------+------------------
        5 | AAAAAAAABAAAAAAA
   (1 row)

Example of the **INTERSECT** clause: Query records whose **r_reason_id** is **AAAAAAAABAAAAAAA** and whose **r_reason_sk** is smaller than **5**.

::

   SELECT * FROM tpcds.reason_p WHERE r_reason_id='AAAAAAAABAAAAAAA' INTERSECT SELECT * FROM tpcds.reason_p WHERE r_reason_sk<5;
    r_reason_sk |   r_reason_id    |     r_reason_desc
   -------------+------------------+------------------------------------
              4 | AAAAAAAABAAAAAAA | reason 3
              3 | AAAAAAAABAAAAAAA | reason 1
   (2 rows)

Example of the **EXCEPT** clause: Query records whose **r_reason_id** is **AAAAAAAABAAAAAAA** and whose **r_reason_sk** is greater than or equal to **4**.

::

   SELECT * FROM tpcds.reason_p WHERE r_reason_id='AAAAAAAABAAAAAAA' EXCEPT SELECT * FROM tpcds.reason_p WHERE r_reason_sk<4;
   r_reason_sk |   r_reason_id    |      r_reason_desc
   -------------+------------------+------------------------------------
             10 | AAAAAAAABAAAAAAA | reason 2
             10 | AAAAAAAABAAAAAAA | reason 5
             10 | AAAAAAAABAAAAAAA | reason 4
              4 | AAAAAAAABAAAAAAA | reason 3
   (4 rows)

Specify the operator (+) in the **WHERE** clause to indicate a left join.

::

   select t1.sr_item_sk ,t2.c_customer_id from store_returns t1, customer t2 where t1.sr_customer_sk  = t2.c_customer_sk(+)
   order by 1 desc limit 1;
    sr_item_sk | c_customer_id
   ------------+---------------
         18000 |
   (1 row)

Specify the operator (+) in the **WHERE** clause to indicate a right join.

::

   select t1.sr_item_sk ,t2.c_customer_id from store_returns t1, customer t2 where t1.sr_customer_sk(+)  = t2.c_customer_sk
   order by 1 desc limit 1;
    sr_item_sk |  c_customer_id
   ------------+------------------
               | AAAAAAAAJNGEBAAA
   (1 row)

Specify the operator (+) in the **WHERE** clause to indicate a left join and add a join condition.

::

   select t1.sr_item_sk ,t2.c_customer_id from store_returns t1, customer t2 where t1.sr_customer_sk  = t2.c_customer_sk(+) and t2.c_customer_sk(+) < 1 order by 1  limit 1;
    sr_item_sk | c_customer_id
   ------------+---------------
             1 |
   (1 row)

If the operator (+) is specified in the **WHERE** clause, do not use expressions connected through **AND**/**OR**.

::

   select t1.sr_item_sk ,t2.c_customer_id from store_returns t1, customer t2 where not(t1.sr_customer_sk  = t2.c_customer_sk(+) and t2.c_customer_sk(+) < 1);
   ERROR:  Operator "(+)" can not be used in nesting expression.
   LINE 1: ...tomer_id from store_returns t1, customer t2 where not(t1.sr_...

If the operator (+) is specified in the **WHERE** clause which does not support expression macros, an error will be reported.

::

   select t1.sr_item_sk ,t2.c_customer_id from store_returns t1, customer t2 where (t1.sr_customer_sk  = t2.c_customer_sk(+))::bool;
   ERROR:  Operator "(+)" can only be used in common expression.

If the operator (+) is specified on both sides of an expression in the **WHERE** clause, an error will be reported.

::

   select t1.sr_item_sk ,t2.c_customer_id from store_returns t1, customer t2 where t1.sr_customer_sk(+)  = t2.c_customer_sk(+);
   ERROR:  Operator "(+)" can't be specified on more than one relation in one join condition
   HINT:  "t1", "t2"...are specified Operator "(+)" in one condition.
