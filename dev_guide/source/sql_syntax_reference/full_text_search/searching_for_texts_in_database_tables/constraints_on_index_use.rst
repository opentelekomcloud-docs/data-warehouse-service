:original_name: dws_06_0090.html

.. _dws_06_0090:

Constraints on Index Use
========================

The following is an example of using an index. Run the following statements in a database that uses the UTF-8 or GBK encoding:

::

   CREATE TBALE table1 (c_int int,c_bigint bigint,c_varchar varchar,c_text text) with(orientation=row);

   CREATE TEXT SEARCH CONFIGURATION ts_conf_1(parser=POUND);
   CREATE TEXT SEARCH CONFIGURATION ts_conf_2(parser=POUND) with(split_flag='%');

   SET default_text_search_config='ts_conf_1';
   CREATE INDEX idx1 ON table1 using gin(to_tsvector(c_text));

   SET default_text_search_config='ts_conf_2';
   CREATE INDEX idx2 ON table1 using gin(to_tsvector(c_text));

   SELECT c_varchar,to_tsvector(c_varchar) FROM table1 where to_tsvector(c_text) @@ plainto_tsquery('¥#@...&**')   and to_tsvector(c_text) @@ plainto_tsquery('Company ')   and c_varchar is not null order by 1 desc limit 3;

In this example, **table1** has two GIN indexes created on the same column **c_text**, **idx1** and **idx2**, but these two indexes are created under different settings of **default_text_search_config**. Differences between this example and the scenario where one table has common indexes created on the same column are as follows:

-  GIN indexes use different parsers (that is, different delimiters). In this case, the index data of **idx1** is different from that of **idx2**.
-  In the specified scenario, the index data of multiple common indexes created on the same column is the same.

As a result, using **idx1** and **idx2** for the same query returns different results.

Constraints
-----------

Still use the above example. When:

-  Multiple GIN indexes are created on the same column of the same table.
-  The GIN indexes use different parsers (that is, different delimiters).
-  The column is used in a query, and an index scan is used in the execution plan.

To avoid different query results caused by different GIN indexes, ensure that only one GIN index is available on a column of the physical table.
