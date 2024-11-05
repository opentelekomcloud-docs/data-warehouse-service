:original_name: dws_06_0021.html

.. _dws_06_0021:

HLL Data Types
==============

HyperLoglog (HLL) is an approximation algorithm for efficiently counting the number of distinct values in a data set. It features faster computing and lower space usage. You only need to store HLL data structures, instead of data sets. When new data is added to a data set, make hash calculation on the data and insert the result to an HLL. Then, you can obtain the final result based on the HLL.

:ref:`Table 1 <en-us_topic_0000001702130117__en-us_topic_0000001233708675_table55621821164213>` compares HLL with other algorithms.

.. _en-us_topic_0000001702130117__en-us_topic_0000001233708675_table55621821164213:

.. table:: **Table 1** Comparison between HLL and other algorithms

   ========================= ================= ================ ==========
   Item                      Sorting Algorithm Hash Algorithm   HLL
   ========================= ================= ================ ==========
   Time complexity           O(nlogn)          O(n)             O(n)
   Space complexity          O(n)              O(n)             1280 bytes
   Error rate                0                 0                ≈2%
   Storage space requirement Size of raw data  Size of raw data 1280 bytes
   ========================= ================= ================ ==========

HLL has advantages over others in the computing speed and storage space requirement. In terms of time complexity, the sorting algorithm needs O(nlogn) time for sorting, and the hash algorithm and HLL need O(n) time for full table scanning. In terms of storage space requirements, the sorting algorithm and hash algorithm need to store raw data before collecting statistics, whereas the HLL algorithm needs to store only the HLL data structures rather than the raw data, and thereby occupying a fixed space of only 1280 bytes.

.. important::

   -  In default specifications, the maximum number of distinct values is 1.6e plus 12, and the maximum error rate is only 2.3%. If a calculation result exceeds the maximum number, the error rate of the calculation result will increase, or the calculation will fail and an error will be reported.
   -  When using this feature for the first time, you need to evaluate the distinct values of the service, properly select configuration parameters, and perform verification to ensure that the accuracy meets requirements.

      -  When default parameter configuration is used, the calculated number of distinct values is 1.6e plus 12. If the calculated result is **NaN**, you need to adjust **log2m** and **regwidth** to accommodate more distinct values.
      -  The hash algorithm has an extremely low probability of collision. However, you are still advised to select 2 or 3 hash seeds for verification when using the hash algorithm for the first time. If there is only a small difference between the distinct values, you can select any one of the seeds as the hash seed.

:ref:`Table 2 <en-us_topic_0000001702130117__en-us_topic_0000001233708675_table18186113885012>` describes main HLL data structures.

.. _en-us_topic_0000001702130117__en-us_topic_0000001233708675_table18186113885012:

.. table:: **Table 2** Main HLL data structures

   +-----------+-------------------------------------------------------------------------------------------------------+
   | Data Type | Description                                                                                           |
   +===========+=======================================================================================================+
   | hll       | Its size is always 1280 bytes, which can be directly used to calculate the number of distinct values. |
   +-----------+-------------------------------------------------------------------------------------------------------+

Application Scenarios of HLL
----------------------------

-  Using the HLL Data Type

   #. Create an HLL table and insert an empty HLL into the table.

      ::

         CREATE TABLE helloworld (id integer, set hll);
         INSERT INTO helloworld(id, set) VALUES (1, hll_empty());

   #. Add an integer that has gone through hash calculation into to the HLL.

      ::

         UPDATE helloworld SET set = hll_add(set, hll_hash_integer(12345)) WHERE id = 1;

   #. Add a string that has gone through hash calculation into to the HLL.

      ::

         UPDATE helloworld SET set = hll_add(set, hll_hash_text('hello world')) WHERE id = 1;

   #. Obtain the number of distinct values of the HLL.

      ::

         SELECT hll_cardinality(set) FROM helloworld WHERE id = 1;
          hll_cardinality
         -----------------
                        2
         (1 row)

-  Using HLL to Collect Website Visitor Statistics

   #. Create the original data table **facts** to record the time when a user accesses a website.

      ::

         CREATE TABLE facts (
                  date            date,
                  user_id         integer
         );

   #. Insert user visits data.

      ::

         INSERT INTO facts VALUES ('2019-02-20', generate_series(1,100));
         INSERT INTO facts VALUES ('2019-02-21', generate_series(1,200));
         INSERT INTO facts VALUES ('2019-02-22', generate_series(1,300));
         INSERT INTO facts VALUES ('2019-02-23', generate_series(1,400));
         INSERT INTO facts VALUES ('2019-02-24', generate_series(1,500));
         INSERT INTO facts VALUES ('2019-02-25', generate_series(1,600));
         INSERT INTO facts VALUES ('2019-02-26', generate_series(1,700));
         INSERT INTO facts VALUES ('2019-02-27', generate_series(1,800));

   #. Create another table and specify an HLL column: Group data by date and insert the data into the HLL:

      ::

         CREATE TABLE daily_uniques (
             date            date UNIQUE,
             users           hll
         );

         INSERT INTO daily_uniques(date, users)
             SELECT date, hll_add_agg(hll_hash_integer(user_id))
             FROM facts
             GROUP BY 1;

   #. Calculate the numbers of users visiting the website every day:

      ::

         SELECT date, hll_cardinality(users) FROM daily_uniques ORDER BY date;
                 date         | hll_cardinality
         ---------------------+------------------
          2019-02-20 00:00:00 |              100
          2019-02-21 00:00:00 | 203.813355588808
          2019-02-22 00:00:00 | 308.048239950384
          2019-02-23 00:00:00 | 410.529188080374
          2019-02-24 00:00:00 | 513.263875705319
          2019-02-25 00:00:00 | 609.271181107416
          2019-02-26 00:00:00 | 702.941844662509
          2019-02-27 00:00:00 | 792.249946595237
         (8 rows)

   #. Calculate the number of users who had visited the website in the week from February 20, 2019 to February 26, 2019:

      ::

         SELECT hll_cardinality(hll_union_agg(users)) FROM daily_uniques WHERE date >= '2019-02-20'::date AND date <= '2019-02-26'::date;
          hll_cardinality
         ------------------
          702.941844662509
         (1 row)

   #. Calculate the number of users who had visited the website yesterday but have not visited the website today:

      ::

         SELECT date, (#hll_union_agg(users) OVER two_days) - #users AS lost_uniques FROM daily_uniques WINDOW two_days AS (ORDER BY date ASC ROWS 1 PRECEDING);
                 date         | lost_uniques
         ---------------------+--------------
          2019-02-20 00:00:00 |            0
          2019-02-21 00:00:00 |            0
          2019-02-22 00:00:00 |            0
          2019-02-23 00:00:00 |            0
          2019-02-24 00:00:00 |            0
          2019-02-25 00:00:00 |            0
          2019-02-26 00:00:00 |            0
          2019-02-27 00:00:00 |            0
         (8 rows)

-  HLL Format Errors

   When inserting data into a column of the HLL type, ensure that the data meets the requirements of the HLL data structure. If the data does not meet the requirements after being parsed, an error will be reported.

   For example, when **E\\\\1234'** is inserted, the data does not comply with the HLL data structure and cannot be parsed. As a result, an error is reported.

   ::

      CREATE TABLE test(id integer, set hll);
      INSERT INTO test VALUES(1, 'E\\1234');
      ERROR:  invalid input syntax for integer: "E\\1234"
