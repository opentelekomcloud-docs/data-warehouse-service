:original_name: dws_04_0462.html

.. _dws_04_0462:

Skew Hints
==========

Function
--------

Theses hints specify redistribution keys containing skew data and skew values, and are used to optimize redistribution involving Join or HashAgg.

Precautions
-----------

-  Skew hints are used only if redistribution is required and the specified skew information matches the redistribution information.
-  Skew hints are controlled by the GUC parameter :ref:`skew_option <en-us_topic_0000001811609661__section1211182712176>`. If the parameter is disabled, skew hints cannot be used for solving skew.
-  Currently, skew hints support only the table relationships of the ordinary table and subquery types. Hints can be specified for base tables, subqueries, and **WITH ... AS** clauses. Unlike other hints, a subquery can be used in skew hints regardless of whether it is pulled up.
-  Use an alias (if any) to specify a table where data skew occurs.
-  You can use a name or an alias to specify a skew column as long as it is not ambiguous. The columns in skew hints cannot be expressions. If data skew occurs in the redistribution that uses an expression as a redistribution key, set the redistribution key as a new column and specify the column in skew hints.
-  The number of skew values must be an integer multiple of the number of columns. Skew values must be grouped based on the column sequence, with each group containing a maximum of 10 values. You can specify duplicate values to group skew columns having different number of skew values. For example, the **c1** and **c2** columns of the **t1** table contains skew data. The skew value of the **c1** column is **a1**, and the skew values of the **c2** column are **b1** and **b2**. In this case, the skew hint is **skew(t1 (c1 c2) ((a1 b1)(a1 b2)))**. **(a1 b1)** is a value group, where **NULL** is allowed as a skew value. Each hint can contain a maximum of 10 groups and the number of groups should be an integer multiple of the number of columns.
-  In the redistribution optimization of Join, a skew value must be specified for skew hints. The skew value can be left empty for HashAgg.
-  If multiple tables, columns, or values are specified, separate items of the same type with spaces.
-  The type of skew values cannot be forcibly converted in hints. To specify a string, enclose it with single quotation marks (' ').

Syntax
------

-  Specify single-table skew.

   .. code-block:: console

      skew([@block_name] table (column) [(value)])

-  Specify intermediate result skew.

   .. code-block:: console

      skew([@block_name] (join_rel) (column) [(value)])

Parameter Description
---------------------

-  *block_name* indicates the block name of the statement block. For details, see :ref:`block_name <en-us_topic_0000002080515442__en-us_topic_0000001460722632_li99021444551>`.
-  **table** specifies the table where skew occurs.

   .. note::

      -  The syntax format of the table is as follows:

         [schema.]table[@block_name]

         The table name can contain the schema name or block name before the subquery statement block is promoted. If the subquery statement block is optimized and rewritten by the optimizer, the value of **block_name** is different from that of **block_name** in leading.

      -  If a table has an alias, the alias is preferentially used to represent the table.

-  **join_rel** specifies two or more joined tables. For example, **(t1 t2)** indicates that the result of joining **t1** and **t2** tables contains skew data.
-  **column** specifies one or more columns where skew occurs.
-  **value** specifies one or more skew values.

Example:

-  Specify single-table skew.

   Each skew hint describes the skew information of one table relationship. To describe the skews of multiple table relationships in a query, specify multiple skew hints.

   Skew hints have the following formats:

   -  One skew value in one column: **skew(t (c1) (v1))**

      Description: The **v1** value in the **c1** column of the **t** table relationship causes skew in query execution.

   -  Multiple skew values in one column: **skew(t (c1) (v1 v2 v3 ...))**

      Description: Values including **v1,** **v2**, and **v3** in the **c1** column of the **t** table relationship cause skew in query execution.

   -  Multiple columns, each having one skew value: **skew(t (c1 c2) (v1 v2))**

      Description: The **v1** value in the **c1** column and the **v2** value in the **c2** column of the **t** table relationship cause skew in query execution.

   -  Multiple columns, each having multiple skew values: **skew(t (c1 c2) ((v1 v2) (v3 v4) (v5 v6) ...))**

      Description: Values including **v1,** **v3**, and **v5** in the **c1** column and values including **v2,** **v4**, and **v6** in the **c2** column of the **t** table relationship cause skew in query execution.

      .. important::

         In the last format, parentheses for skew value groups can be omitted, for example, **skew(t (c1 c2) (v1 v2 v3 v4 v5 v6 ...))**. In a skew hint, either use parentheses for all skew value groups or for none of them.

         Otherwise, a syntax error will be generated. For example, **skew(t (c1 c2) (v1 v2 v3 v4 (v5 v6) ...))** will generate an error.

-  Specify intermediate result skew.

   If data skew does not occur in base tables but in an intermediate result during query execution, specify skew hints of the intermediate result to solve the skew. The format is **skew((t1 t2) (c1) (v1))**.

   Description: Data skew occurs after the table relationships **t1** and **t2** are joined. The **c1** column of the **t1** table contains skew data and its skew value is **v1**.

   **c1** can exist only in a table relationship of **join_rel**. If there is another column having the same name, use aliases to avoid ambiguity.

Suggestion
----------

-  For a multi-level query, write the hint on the layer where data skew occurs.
-  For a listed subquery, you can specify the subquery name in a hint. If you know data skew occurs on which base table, directly specify the table.
-  Aliases are preferred when you specify a table or column in a hint.

Examples
--------

Specify single-table skew.

-  Specify hints in the original query.

   For example, the original query is as follows:

   ::

      explain
      with customer_total_return as
      (select sr_customer_sk as ctr_customer_sk
      ,sr_store_sk as ctr_store_sk
      ,sum(SR_FEE) as ctr_total_return
      from store_returns
      ,date_dim
      where sr_returned_date_sk = d_date_sk
      and d_year =2000
      group by sr_customer_sk
      ,sr_store_sk)
       select  c_customer_id
      from customer_total_return ctr1
      ,store
      ,customer
      where ctr1.ctr_total_return > (select avg(ctr_total_return)*1.2
      from customer_total_return ctr2
      where ctr1.ctr_store_sk = ctr2.ctr_store_sk)
      and s_store_sk = ctr1.ctr_store_sk
      and s_state = 'NM'
      and ctr1.ctr_customer_sk = c_customer_sk
      order by c_customer_id
      limit 100;

   |image1|

   Specify the hints of HashAgg in the inner **with** clause and of the outer Hash Join. The query containing hints is as follows:

   ::

      explain
      with customer_total_return as
      (select /*+ skew(store_returns(sr_store_sk sr_customer_sk)) */sr_customer_sk as ctr_customer_sk
      ,sr_store_sk as ctr_store_sk
      ,sum(SR_FEE) as ctr_total_return
      from store_returns
      ,date_dim
      where sr_returned_date_sk = d_date_sk
      and d_year =2000
      group by sr_customer_sk
      ,sr_store_sk)
       select  /*+ skew(ctr1(ctr_customer_sk)(11))*/  c_customer_id
      from customer_total_return ctr1
      ,store
      ,customer
      where ctr1.ctr_total_return > (select avg(ctr_total_return)*1.2
      from customer_total_return ctr2
      where ctr1.ctr_store_sk = ctr2.ctr_store_sk)
      and s_store_sk = ctr1.ctr_store_sk
      and s_state = 'NM'
      and ctr1.ctr_customer_sk = c_customer_sk
      order by c_customer_id
      limit 100;

   The hints indicate that the **group by** in the inner **with** clause contains skew data during redistribution by HashAgg, corresponding to the original Hash Agg operators 10 and 21; and that the **ctr_customer_sk** column in the outer **ctr1** table contains skew data during redistribution by Hash Join, corresponding to operator 6 in the original plan. The optimized plan is as follows:

   |image2|

   To solve data skew in the redistribution, Hash Agg is changed to double-level Agg operators and the redistribution operators used by Hash Join are changed in the optimized plan.

-  Modify the query and then specify hints.

   For example, the original query and its plan are as follows:

   ::

      explain select count(*) from store_sales_1 group by round(ss_list_price);

   |image3|

   Columns in hints do not support expressions. To specify hints, rewrite the query as several subqueries. The rewritten query and its plan are as follows:

   ::

      explain
      select count(*)
      from (select round(ss_list_price),ss_hdemo_sk
      from store_sales_1)tmp(a,ss_hdemo_sk)
      group by a;

   |image4|

   Ensure that the service logic is not changed during the rewriting.

   Specify hints in the rewritten query as follows:

   ::

      explain
      select /*+ skew(tmp(a)) */ count(*)
      from (select round(ss_list_price),ss_hdemo_sk
      from store_sales_1)tmp(a,ss_hdemo_sk)
      group by a;

   |image5|

   The plan shows that after Hash Agg is changed to double-layer Agg operators, redistributed data is greatly reduced and redistribution time shortened.

   You can specify hints in columns in a subquery, for example:

   ::

      explain
      select /*+ skew(tmp(b)) */ count(*)
      from (select round(ss_list_price) b,ss_hdemo_sk
      from store_sales_1)tmp(a,ss_hdemo_sk)
      group by a;

.. |image1| image:: /_static/images/en-us_image_0000001460723200.png
.. |image2| image:: /_static/images/en-us_image_0000001510284029.png
.. |image3| image:: /_static/images/en-us_image_0000001510522957.png
.. |image4| image:: /_static/images/en-us_image_0000001510163285.png
.. |image5| image:: /_static/images/en-us_image_0000001460563396.png
