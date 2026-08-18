:original_name: dws_04_0450.html

.. _dws_04_0450:

.. _en-us_topic_0000002052813810:

Tuning Operators
================

Background
----------

A query goes through many steps to produce its final result. Often, the whole query slows down because one step takes too long. This slow step is called a bottleneck. To fix this, you can run the **EXPLAIN ANALYZE** or **PERFORMANCE** command to find the bottleneck.

In the example below, the **Hashagg** operator takes up 66% of the total time: (51016 - 13535)/56476 ≈ 66%. So, the **Hashagg** operator is the bottleneck. Start by optimizing this operator to improve performance.

|image1|

Operator Tuning Example
-----------------------

Example 1: When scanning a base table with SeqScan, filtering large amounts of data, like in point queries or range scans, can be slow. Create an index on the condition column and use IndexScan to speed up the process.

::

    explain (analyze on, costs off) select * from store_sales where ss_sold_date_sk = 2450944;
    id |             operation          |       A-time        | A-rows | Peak Memory  | A-width
   ----+--------------------------------+---------------------+--------+--------------+---------
     1 | ->  Streaming (type: GATHER)   | 3666.020            |   3360 | 195KB        |
     2 |    ->  Seq Scan on store_sales | [3594.611,3594.611] |   3360 | [34KB, 34KB] |

    Predicate Information (identified by plan id)
   -----------------------------------------------
      2 --Seq Scan on store_sales
            Filter: (ss_sold_date_sk = 2450944)
            Rows Removed by Filter: 4968936

::

    create index idx on store_sales_row(ss_sold_date_sk);
   CREATE INDEX
    explain (analyze on, costs off) select * from store_sales_row where ss_sold_date_sk = 2450944;
    id |                   operation                    |     A-time      | A-rows | Peak Memory  | A-width
   ----+------------------------------------------------+-----------------+--------+--------------+----------
     1 | ->  Streaming (type: GATHER)                   | 81.524          |   3360 | 195KB        |
     2 |    ->  Index Scan using idx on store_sales_row | [13.352,13.352] |   3360 | [34KB, 34KB] |

For instance, a full table scan returned 3,360 records but took 3.6 seconds. After indexing the **ss_sold_date_sk** column, the scan time dropped to 13 milliseconds.

Example 2: If **NestLoop** is chosen to join two tables and the row count is high, the join can be slow. In one case, **NestLoop** took 181 seconds. By setting **enable_mergejoin** and **enable_nestloop** to **off** and letting the optimizer choose **HashJoin**, the join time was reduced to over 200 milliseconds.

|image2|

|image3|

Example 3: **HashAgg** usually performs better. For large result sets, if **Sort** and **GroupAgg** are used, set **enable_sort** to off. **HashAgg** takes much longer time than **Sort** and **GroupAgg** used together.

|image4|

|image5|

.. |image1| image:: /_static/images/en-us_image_0000001233563361.jpg
.. |image2| image:: /_static/images/en-us_image_0000001233883415.png
.. |image3| image:: /_static/images/en-us_image_0000001233681853.png
.. |image4| image:: /_static/images/en-us_image_0000001188482338.png
.. |image5| image:: /_static/images/en-us_image_0000001188642252.png
