:original_name: dws_04_0479.html

.. _dws_04_0479:

.. _en-us_topic_0000001579070006:

Case: Configuring cost_param for Better Query Performance
=========================================================

The cost_param parameter is used to control use of different estimation methods in specific customer scenarios, allowing estimated values to be close to onsite values. This parameter can control various methods simultaneously by performing AND (&) operations on the bit for each method. A method is selected if its value is not **0**.

Scenario 1: Before Optimization
-------------------------------

If **bit0** of **cost_param** is set to **1**, an improved mechanism is used for estimating the selection rate of non-equi-joins. This method is more accurate for estimating the selection rate of joins between two identical tables. The following example describes the optimization scenario when **bit0** of **cost_param** is set to **1**. In V300R002C00 and later, **cost_param & 1=0** is not used. That is, an optimized formula is selected for calculation.

.. note::

   The selection rate indicates the percentage for which the number of rows meeting the join conditions account of the **JOIN** results when the **JOIN** relationship is established between two tables.

The table structure is as follows:

::

   CREATE TABLE LINEITEM
   (
   L_ORDERKEY BIGINT NOT NULL
   , L_PARTKEY BIGINT NOT NULL
   , L_SUPPKEY BIGINT NOT NULL
   , L_LINENUMBER BIGINT NOT NULL
   , L_QUANTITY DECIMAL(15,2) NOT NULL
   , L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL
   , L_DISCOUNT DECIMAL(15,2) NOT NULL
   , L_TAX DECIMAL(15,2) NOT NULL
   , L_RETURNFLAG CHAR(1) NOT NULL
   , L_LINESTATUS CHAR(1) NOT NULL
   , L_SHIPDATE DATE NOT NULL
   , L_COMMITDATE DATE NOT NULL
   , L_RECEIPTDATE DATE NOT NULL
   , L_SHIPINSTRUCT CHAR(25) NOT NULL
   , L_SHIPMODE CHAR(10) NOT NULL
   , L_COMMENT VARCHAR(44) NOT NULL
   ) with (orientation = column, COMPRESSION = MIDDLE) distribute by hash(L_ORDERKEY);

   CREATE TABLE ORDERS
   (
   O_ORDERKEY BIGINT NOT NULL
   , O_CUSTKEY BIGINT NOT NULL
   , O_ORDERSTATUS CHAR(1) NOT NULL
   , O_TOTALPRICE DECIMAL(15,2) NOT NULL
   , O_ORDERDATE DATE NOT NULL
   , O_ORDERPRIORITY CHAR(15) NOT NULL
   , O_CLERK CHAR(15) NOT NULL
   , O_SHIPPRIORITY BIGINT NOT NULL
   , O_COMMENT VARCHAR(79) NOT NULL
   )with (orientation = column, COMPRESSION = MIDDLE) distribute by hash(O_ORDERKEY);

The query statements are as follows:

::

   explain verbose select
   count(*) as numwait
   from
   lineitem l1,
   orders
   where
   o_orderkey = l1.l_orderkey
   and o_orderstatus = 'F'
   and l1.l_receiptdate > l1.l_commitdate
   and not exists (
   select
   *
   from
   lineitem l3
   where
   l3.l_orderkey = l1.l_orderkey
   and l3.l_suppkey <> l1.l_suppkey
   and l3.l_receiptdate > l3.l_commitdate
   )
   order by
   numwait desc;

The following figure shows the execution plan. (When **verbose** is used, **distinct** is added for column selection which is controlled by **cost off/on**. The hash join rows show the estimated number of distinct values and the other rows do not.)

|image1|

Scenario 1: After Optimization
------------------------------

These queries are from Anti Join connected in the **lineitem** table. When **cost_param & bit0** is **0**, the estimated number of Anti Join rows greatly differs from that of the actual number of rows, compromising the query performance. You can estimate the number of Anti Join rows more accurately by setting **cost_param & bit0** to **1** to improve the query performance. The optimized execution plan is as follows:

|image2|

Scenario 2: Before Optimization
-------------------------------

If **bit1** is set to **1** (**set cost_param=2**), the selection rate is estimated based on multiple filter criteria. The lowest selection rate among all filter criteria, but not the product of the selection rates for two tables under a specific filter criterion, is used as the total selection rate. This method is more accurate when a close correlation exists between the columns to be filtered. The following example describes the optimization scenario when **bit1** of **cost_param** is set to **1**.

The table structure is as follows:

::

   CREATE TABLE NATION
   (
   N_NATIONKEYINT NOT NULL
   , N_NAMECHAR(25) NOT NULL
   , N_REGIONKEYINT NOT NULL
   , N_COMMENTVARCHAR(152)
   ) distribute by replication;
   CREATE TABLE SUPPLIER
   (
   S_SUPPKEYBIGINT NOT NULL
   , S_NAMECHAR(25) NOT NULL
   , S_ADDRESSVARCHAR(40) NOT NULL
   , S_NATIONKEYINT NOT NULL
   , S_PHONECHAR(15) NOT NULL
   , S_ACCTBALDECIMAL(15,2) NOT NULL
   , S_COMMENTVARCHAR(101) NOT NULL
   ) distribute by hash(S_SUPPKEY);
   CREATE TABLE PARTSUPP
   (
   PS_PARTKEYBIGINT NOT NULL
   , PS_SUPPKEYBIGINT NOT NULL
   , PS_AVAILQTYBIGINT NOT NULL
   , PS_SUPPLYCOSTDECIMAL(15,2)NOT NULL
   , PS_COMMENTVARCHAR(199) NOT NULL
   )distribute by hash(PS_PARTKEY);

The query statements are as follows:

::

   set cost_param=2;
   explain verbose select
   nation,
   sum(amount) as sum_profit
   from
   (
   select
   n_name as nation,
   l_extendedprice * (1 - l_discount) - ps_supplycost * l_quantity as amount
   from
   supplier,
   lineitem,
   partsupp,
   nation
   where
   s_suppkey = l_suppkey
   and ps_suppkey = l_suppkey
   and ps_partkey = l_partkey
   and s_nationkey = n_nationkey
   ) as profit
   group by nation
   order by nation;

When **bit1** of **cost_param** is **0**, the execution plan is shown as follows:

|image3|

Scenario 2: After Optimization
------------------------------

In the preceding queries, the hash join criteria of the supplier, lineitem, and partsupp tables are setting **lineitem.l_suppkey** to **supplier.s_suppkey** and **lineitem.l_partkey** to **partsupp.ps_partkey**. Two filter criteria exist in the hash join conditions. **lineitem.l_suppkey** in the first filter criteria and **lineitem.l_partkey** in the second filter criteria are two columns with strong relationship of the lineitem table. In this situation, when you estimate the rate of the hash join conditions, if **cost_param & bit1** is **0**, the selection rate is estimated based on multiple filter criteria. The lowest selection rate among all filter criteria, but not the product of the selection rates for two tables under a specific filter criterion, is used as the total selection rate. This method is more accurate when a close correlation exists between the columns to be filtered. The plan after optimization is shown as follows:

|image4|

.. |image1| image:: /_static/images/en-us_image_0000001188163834.png
.. |image2| image:: /_static/images/en-us_image_0000001233681879.png
.. |image3| image:: /_static/images/en-us_image_0000001233563387.png
.. |image4| image:: /_static/images/en-us_image_0000001233883443.png
