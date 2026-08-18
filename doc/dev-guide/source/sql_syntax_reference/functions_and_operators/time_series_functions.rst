:original_name: dws_06_0294.html

.. _dws_06_0294:

Time Series Functions
=====================

:ref:`Table 1 <en-us_topic_0000002499002873__table1226195117447>` lists the time series calculation functions supported by DWS.

.. _en-us_topic_0000002499002873__table1226195117447:

.. table:: **Table 1** Time series calculation functions

   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Time Series Calculation Function                                                                                                                                                                                       | Description                                                                                                                                                                          |
   +========================================================================================================================================================================================================================+======================================================================================================================================================================================+
   | :ref:`delta(field numeric) <en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_en-us_topic_0000001147483463_en-us_topic_0237121449_section1912375517369>`                         | Calculates the difference between two rows sorted by time.                                                                                                                           |
   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`spread(field numeric) <en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_en-us_topic_0000001147483463_en-us_topic_0237121449_section18749154224310>`                       | Calculates the difference between the maximum value and the minimum value in a specified period.                                                                                     |
   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`mode() within group (order by value anyelement) <en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_en-us_topic_0000001147483463_en-us_topic_0237121449_section6591255549>` | Returns the value with the highest occurrence frequency for a given column. If multiple values have the same frequency, this function returns the smallest value among these values. |
   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`value_of_percentile(column float, percentile float, compression float) <en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_section143565195410>`                            | Calculates the percentile. This is the approximation algorithm of **percentile_cont**.                                                                                               |
   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`percentile_of_value(column float, percentilevalue float, compression float) <en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_section5593144914120>`                      | Calculates a value based on a given percentile. This is the inverse function of **value_of_percentile**.                                                                             |
   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`first(column1, column2) <en-us_topic_0000002499002873__en-us_topic_0000001209452030_section23182210593>`                                                                                                         | Compares the values of **column2** in a group, finds the minimum value, and outputs the value of **column1**.                                                                        |
   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`last(column1, column2) <en-us_topic_0000002499002873__en-us_topic_0000001209452030_section1486814516014>`                                                                                                        | Compares the values of **column2** in a group, finds the maximum value, and outputs the corresponding value of **column1**.                                                          |
   +------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_en-us_topic_0000001147483463_en-us_topic_0237121449_section1912375517369:

delta(field numeric)
--------------------

Description: Calculates the difference between two adjacent rows sorted by time in time series scenarios. It is used to monitor metrics such as traffic and speed.

Parameter: **field** indicates the column to be calculated.

.. caution::

   The **delta** window function needs to be used together with the **over** window function. The **rows** statement in the **over** statement does not change the result of the **delta** function. For example, the results returned by **delta(value) over(order by time rows 1 preceding)** and **delta(value) over(order by time rows 3 preceding)** are the same.

Example:

::

   SELECT
        delta(value) over (rows 1 preceding)
   FROM
        (VALUES ('2019-07-12 00:00:00'::timestamptz, 1),
                ('2019-07-12 00:01:00'::timestamptz, 3),
                ('2019-07-12 00:02:00'::timestamptz, 7))
        v(time,value) order by 1;
    delta
   -------
        0
        2
        4
   (3 rows)

.. _en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_en-us_topic_0000001147483463_en-us_topic_0237121449_section18749154224310:

spread(field numeric)
---------------------

Description: Calculates the increment of each metric in a group. Generally, the increment is calculated after the metric is sorted by time.

Parameter: **field** indicates the column to be calculated.

.. caution::

   If there are less than two tuples in each group, the returned result is **0**. Do not use this function with the **over** window function.

Example:

::

   SELECT
        time, SPREAD(value)
    FROM
        (VALUES ('2019-07-12 00:00:00'::timestamptz, 1), ('2019-07-12 00:00:00'::timestamptz, 3),
                ('2019-07-12 00:01:00'::timestamptz, 3), ('2019-07-12 00:01:00'::timestamptz, 6),
                ('2019-07-12 00:02:00'::timestamptz, 7), ('2019-07-12 00:02:00'::timestamptz, 9))
        v(time,value) group by time order by time;
             time          | spread
   ------------------------+--------
    2019-07-12 00:00:00+08 |      2
    2019-07-12 00:01:00+08 |      3
    2019-07-12 00:02:00+08 |      2
   (3 rows)

.. _en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_en-us_topic_0000001147483463_en-us_topic_0237121449_section6591255549:

mode() within group (order by value anyelement)
-----------------------------------------------

Description: Returns the value with the highest occurrence frequency for a given column. If multiple values have the same frequency, this function returns the smallest value among these values.

Parameter: **value** indicates the column to be queried.

.. caution::

   -  This function must be used together with the **WITHIN GROUP** statement, or an error will be reported. The parameter of this function is placed after **ORDER BY** of the group.
   -  This function cannot be used together with the **over** clause.

Example:

::

   SELECT
        mode() within group (order by value)
    FROM
        (VALUES ('2019-07-12 00:00:00'::timestamptz, 1),
                ('2019-07-12 00:01:00'::timestamptz, 1),
                ('2019-07-12 00:02:00'::timestamptz, 7))
        v(time,value);
    mode
   ------
       1
   (1 row)

.. _en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_section143565195410:

value_of_percentile(column float, percentile float, compression float)
----------------------------------------------------------------------

Description: Returns an approximate percentile value for a given column in ascending order. It is an approximate result of **percentile_cont**, but the performance is better than that of **percentile_cont**.

.. table:: **Table 2** Parameters

   +-------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Type  | Description                                                                                                                                                                                                                                                                                              |
   +=============+=======+==========================================================================================================================================================================================================================================================================================================+
   | column      | float | Column whose percentile is to be calculated                                                                                                                                                                                                                                                              |
   +-------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | percentile  | float | Percentile value. Value range: **0** to **1**                                                                                                                                                                                                                                                            |
   +-------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compression | float | Compression coefficient. The value ranges from **0** to **500**. The default value is **300**. A larger value indicates that more memory is occupied during function calculation and the result precision is higher. If the specified value is not within the value range, the value is regarded as 300. |
   +-------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   SELECT value_of_percentile(values, 0.8, 0) from TABLE;

.. _en-us_topic_0000002499002873__en-us_topic_0000001209452030_en-us_topic_0000001100844348_section5593144914120:

percentile_of_value(column float, percentilevalue float, compression float)
---------------------------------------------------------------------------

Description: Returns the percentile of a given column in ascending order. This function is the inverse function of **value_of_percentile**.

.. table:: **Table 3** Parameters

   +-----------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Type  | Description                                                                                                                                                                                                                                                                                              |
   +=================+=======+==========================================================================================================================================================================================================================================================================================================+
   | column          | float | Column whose percentile is to be calculated                                                                                                                                                                                                                                                              |
   +-----------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | percentilevalue | float | Value whose percentile is to be calculated                                                                                                                                                                                                                                                               |
   +-----------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | compression     | float | Compression coefficient. The value ranges from **0** to **500**. The default value is **300**. A larger value indicates that more memory is occupied during function calculation and the result precision is higher. If the specified value is not within the value range, the value is regarded as 300. |
   +-----------------+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   SELECT percentile_of_value(values, 80, 0) from TABLE;

.. _en-us_topic_0000002499002873__en-us_topic_0000001209452030_section23182210593:

first(column1, column2)
-----------------------

Description: Compares the values of **column2** in a group, finds the minimum value, and outputs the value of **column1**. It is an aggregation function.

.. table:: **Table 4** Parameters

   ========= ============================= =================
   Parameter Type                          Description
   ========= ============================= =================
   column1   bigint/text/double/numeric    Output column
   column2   timestamp/timestamptz/numeric Comparison column
   ========= ============================= =================

Example: For details about the sample data, see the table definition and data in the :ref:`time_fill() <en-us_topic_0000002499002873__section201401052154016>` expression.

Obtain the first idle value in each group sorted by time based on **scope_name**.

::

   select first(idle, time_string) from dcs_cpu group by scope_name;
    first
   -------
        1
        3
   (2 rows)

.. _en-us_topic_0000002499002873__en-us_topic_0000001209452030_section1486814516014:

last(column1, column2)
----------------------

Description: Compares the values of **column2** in a group, finds the maximum value, and outputs the value of **column1**. It is an aggregation function.

.. table:: **Table 5** Parameters

   ========= ============================= =================
   Parameter Type                          Description
   ========= ============================= =================
   column1   bigint/text/double/numeric    Output column
   column2   timestamp/timestamptz/numeric Comparison column
   ========= ============================= =================

Example: For details about the sample data, see the table definition and data in the :ref:`time_fill() <en-us_topic_0000002499002873__section201401052154016>` expression.

Obtain the last idle value in each group sorted by time based on **scope_name**.

::

   select last(idle, time_string) from dcs_cpu group by scope_name;
    last
   ------
       3
       2
   (2 rows)

Time Series Filling Functions
-----------------------------

:ref:`Table 6 <en-us_topic_0000002499002873__table164164523819>` lists the time series filling functions supported by DWS.

.. _en-us_topic_0000002499002873__table164164523819:

.. table:: **Table 6** Time series filling functions

   +----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | Time Series Filling Expression                                                                     | Description                                                                 |
   +====================================================================================================+=============================================================================+
   | time_fill(interval, time_column, start_time, end_time), **fill_last**\ (agg_function(agg_column))  | Adds missing time data (aggregation result) after sorting by time column.   |
   |                                                                                                    |                                                                             |
   |                                                                                                    | Uses the former value sorted by time to fill the latter value.              |
   +----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | time_fill(interval, time_column, start_time, end_time), **fill_first**\ (agg_function(agg_column)) | Adds missing time data (aggregation result) after sorting by time column.   |
   |                                                                                                    |                                                                             |
   |                                                                                                    | Uses the latter value sorted by time to fill the former value.              |
   +----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------+
   | time_fill(interval, time_column, start_time, end_time), **fill_avg**\ (agg_function(agg_column))   | Adds missing time data (aggregation result) after sorting by time column.   |
   |                                                                                                    |                                                                             |
   |                                                                                                    | Uses the former and latter values sorted by time to fill the current value. |
   +----------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------+

.. caution::

   -  The **time_fill** function needs to be used as an aggregation function. The **GROUP BY** clause needs to reference its own calculation result and cannot be nested with itself. The clause cannot be called multiple times in a single query, used as a lower-layer computing node, or used with the **WITHIN GROUP** clause.
   -  **time_fill** must be used together with **fill_avg**, **fill_first**, **fill_last**, or the **Agg** function.
   -  **time_fill** can only be used in **GROUP BY**. A **GROUP BY** clause that contains **time_fill** cannot contain other columns.
   -  **time_fill** cannot be used in other positions following SELECT, for example, after **WHERE** or other join conditions.
   -  The **start** timestamp must be smaller than the **finish** timestamp, and their interval must be greater than the value of **window_width**.
   -  All parameters cannot be null. Values of **start** and **finish** must be specified.

.. table:: **Table 7** Parameters

   +--------------------------+---------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | Parameter                | Type                                                                                  | Description                                                     |
   +==========================+=======================================================================================+=================================================================+
   | interval                 | INTERVAL. The minimum interval is 1 second.                                           | Interval grouped by time                                        |
   +--------------------------+---------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | time_column              | TIMESTAMP or TIMESTAMPTZ                                                              | Interval grouped by a specified column                          |
   +--------------------------+---------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | start_time               | TIMESTAMP or TIMESTAMPTZ                                                              | Start time of a group                                           |
   +--------------------------+---------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | end_time                 | TIMESTAMP or TIMESTAMPTZ                                                              | End time of a group                                             |
   +--------------------------+---------------------------------------------------------------------------------------+-----------------------------------------------------------------+
   | agg_function(agg_column) | Specifies the agg function to aggregate specified columns, for example, **max(col)**. | Fills the agg result according to the specified filling method. |
   +--------------------------+---------------------------------------------------------------------------------------+-----------------------------------------------------------------+

.. _en-us_topic_0000002499002873__section201401052154016:

time_fill(interval, time_column, start_time, end_time), fill_last(agg_function(agg_column))
-------------------------------------------------------------------------------------------

Description: Uses the former value sorted by time to fill in the latter value.

Create the sample table **dcs_cpu** and insert data into the table.

::

   create table dcs_cpu(
   idle real,
   vcpu_num int,
   node text,
   scope_name text,
   server_ip text,
   iowait numeric,
   time_string timestamp
   )with (orientation=column) distribute by hash(node);
   insert  into dcs_cpu VALUES(1.0,1,'node_a','scope_a','1.1.1.1',1.0,'2019-07-12 00:10:10');
   insert  into dcs_cpu VALUES(2.0,2,'node_b','scope_a','1.1.1.2',2.0,'2019-07-12 00:12:10');
   insert  into dcs_cpu VALUES(3.0,3,'node_c','scope_b','1.1.1.3',3.0,'2019-07-12 00:13:10');

Calculate the average value in a group at the interval of 1 min. Use the value of the former time segment to fill in the value of the latter time segment.

::

   select time_fill(interval '1 min',time_string,'2019-07-12 00:09:00','2019-07-12 00:14:00'), fill_last(avg(idle)) from dcs_cpu group by time_fill order by time_fill;
         time_fill      | fill_last
   ---------------------+-----------
    2019-07-12 00:09:00 |
    2019-07-12 00:10:00 |         1
    2019-07-12 00:11:00 |         1
    2019-07-12 00:12:00 |         2
    2019-07-12 00:13:00 |         3
    2019-07-12 00:14:00 |         3
   (6 rows)

time_fill(interval, time_column, start_time, end_time), fill_first(agg_function(agg_column))
--------------------------------------------------------------------------------------------

Description: Uses the latter value sorted by time to fill in the former value.

Example:

Calculate the average value in a group at the interval of 1 min. Use the value of the latter time segment to fill in the value of the former time segment. (For details about the sample data, see the table definition and data in :ref:`time_fill(interval, time_column, start_time, end_time), fill_last(agg_function(agg_column)) <en-us_topic_0000002499002873__section201401052154016>`.)

::

   select time_fill(interval '1 min',time_string,'2019-07-12 00:09:00','2019-07-12 00:14:00'), fill_first(avg(idle)) from dcs_cpu group by time_fill order by time_fill;
         time_fill      | fill_first
   ---------------------+------------
    2019-07-12 00:09:00 |          1
    2019-07-12 00:10:00 |          1
    2019-07-12 00:11:00 |          2
    2019-07-12 00:12:00 |          2
    2019-07-12 00:13:00 |          3
    2019-07-12 00:14:00 |
   (6 rows)

time_fill(interval, time_column, start_time, end_time), fill_avg(agg_function(agg_column))
------------------------------------------------------------------------------------------

Description: Uses the former and latter values sorted by time to fill in the current value.

Example:

Calculate the average value in a group at the interval of 1 min. Use the weighted average value of the former and latter values to fill in the current value. (For details about the sample data, see the table definition and data in :ref:`time_fill(interval, time_column, start_time, end_time), fill_last(agg_function(agg_column)) <en-us_topic_0000002499002873__section201401052154016>`.)

::

   select time_fill(interval '1 min',time_string,'2019-07-12 00:09:00','2019-07-12 00:14:00'), fill_avg(avg(idle)) from dcs_cpu group by time_fill order by time_fill;
         time_fill      | fill_avg
   ---------------------+----------
    2019-07-12 00:09:00 |        1
    2019-07-12 00:10:00 |        1
    2019-07-12 00:11:00 |      1.5
    2019-07-12 00:12:00 |        2
    2019-07-12 00:13:00 |        3
    2019-07-12 00:14:00 |        3
   (6 rows)
