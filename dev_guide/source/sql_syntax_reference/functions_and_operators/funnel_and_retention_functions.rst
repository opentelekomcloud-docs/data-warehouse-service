:original_name: dws_06_0372.html

.. _dws_06_0372:

Funnel and Retention Functions
==============================

Funnel and retention functions are supported only in cluster 8.3.0 and later versions.

Context
-------

Both funnel functions and retention functions are widely used tools for analyzing user behavior in product and data analysis. They are particularly useful for product managers, data scientists, and marketing personnel. These functions assist in analyzing the customer journey, comprehending user churn and conversion problems, and assessing the product's ability to consistently attract and retain customers.

-  Funnel analysis: examines each step a customer takes from initial contact to final purchase. It helps product teams and marketers understand how users interact with products or services, as well as the conversion and churn rates at each step. The funnel analysis involves designating stages of the customer's journey from first hearing about a product to making a purchase. The whole process may include visiting a website, registering an account, and purchasing a product.
-  Retention analysis: focuses on the percentage of users who continue using a product after a specified period of time. It calculates the number of users who remain active after a certain time point, such as user registration, first order placement, or initial use. Common retention analysis functions include daily, weekly, and monthly retention rates, which help analyze whether users continue using the product after initially hearing about it.

WINDOW_FUNNEL
-------------

The **WINDOW_FUNNEL** function searches for an event chain in the sliding time window and counts the maximum number of consecutive events in the event chain.

Given a list of user-defined events, GaussDB(DWS) finds the longest sequential match starting from the first event and returns the length of the match. Once the matching fails, the entire matching ends. Example:

Assume that the window is large enough:

-  The condition events are c1, c2, and c3, but the user data is c1, c2, c3, and c4. So, c1, c2, and c3 are matched, and the return value of the function is 3.
-  The condition events are c1, c2, and c3, but the user data is c4, c3, c2, and c1. So, c1 is matched, and the return value of the function is 1.
-  The condition events are c1, c2, and c3, but the user data is c4 and c3. So, no event is matched, and the return value of the function is 0.

**Function syntax**

::

   windowFunnel(window, mode, timestamp, cond1, cond2, ..., condN)

**Input parameters**

-  **window**: bigint type. It indicates the size of the sliding time window. The unit is second.
-  **mode**: text type. Currently, only the **Default** mode is supported. For other modes, an error is reported. In the **Default** mode, it matches as many events as possible, starting from the first event in the window.
-  **timestamp**: time range when an event occurs. The **timestamp without time zone**, **timestamp with time zone**, **date**, **int** and **bigint** types are supported.
-  **cond**: variable-length Boolean array. It indicates the condition. GaussDB(DWS) supports only 1 to 32 conditions. If the number of conditions is not within the range, an error is reported.

**Return values**

**level**: int type. It indicates the maximum length of the event list that matches the condition.

Retention Functions
-------------------

The retention function evaluates if an event meets each condition, starting from the first one.

**Function syntax**

::

   retention(cond1, cond2, ..., cond32);

**Input parameters**

**cond**: variable-length Boolean array with a maximum length of 32 characters, indicating whether an event meets specific conditions. GaussDB(DWS) supports only 1 to 32 conditions. If the number of conditions is not within the range, an error is reported.

**Return values**

**retention condition**: tinyint array type. It indicates the expression of the returned result, which is the same as the length of the input parameter **cond**. If the **cond1** and **condi** conditions are met, the ith value of the returned result is **1**. Otherwise, the ith value is **0**.

Other Retention-Related Functions
---------------------------------

GaussDB(DWS) supports functions **range_retention_count** and **range_retention_sum** as supplements to the retention function for better customer retention analysis.

-  **range_retention_count**

   **range_retention_count** calculates the retention rate of each user. This function returns an array, which can be used as the input parameter of the **range_retention_sum** function.

   **Function syntax**

   ::

      range_retention_count(is_first, is_active, dt, retention_interval, retention_granularity, output_format)

   **Input parameters**

   -  **is_first**: indicates whether it is an initial behavior. The value is of the Boolean type. The value **true** indicates that it is an initial behavior, and the value **false** indicates that it is not an initial behavior.
   -  **is_active**: indicates whether it is a retention behavior. The value is of the Boolean type. The value **true** indicates that it is a retention behavior, and the value **false** indicates that it is not a retention behavior.
   -  **dt**: indicates the date when the behavior happens. The value is of the date type.
   -  **retention_interval**: indicates the storage interval. The value is of the array type. A maximum of 15 storage intervals are supported. For example, ARRAY[1,3,5,7,15,30].
   -  **retention_granularity**: indicates the retention granularity, which can be day, week, or month. The value is of the text type.
   -  **output_format**: indicates the output format. The value is of the text type and can be **normal** (default) or **expand** (daily retention details can be obtained).

   **Return value**: BIGINT array of user retention information.

-  **range_retention_sum**

   **range_retention_sum** summarizes and calculates the daily (weekly/monthly) retention rate of all users.

   **Function syntax**

   ::

      range_retention_sum(range_retention_count_result)

   **Input parameter**: return value of the **range_retention_count** function.

   **Return value**: text array of user retention statistics.

Examples
--------

Create the **funnel_test** table.

.. code-block::

   CREATE TABLE IF NOT EXISTS funnel_test
   (
       user_id INT ,
       event_type TEXT,
       event_time TIMESTAMP,
       event_timez TIMESTAMP WITH TIME ZONE,
       event_time_int BIGINT
   );

Insert data.

.. code-block::

   INSERT INTO funnel_test VALUES
   (1,'Browse','2021-01-31 11:00:00', '2021-01-31 11:00:00+08', 10),
   (1,'Click','2021-01-31 11:10:00', '2021-01-31 11:10:00+07', 20),
   (1,'Order','2021-01-31 11:20:00', '2021-01-31 11:20:00+06', 30),
   (1,'Pay','2021-01-31 11:30:00', '2021-01-31 11:30:00+05', 40),
   (2,'Order','2021-01-31 11:00:00', '2021-01-31 11:00:00+08', 11),
   (2,'Pay','2021-01-31 11:10:00', '2021-01-31 11:10:00+08', 12),
   (1,'Browse','2021-01-31 11:00:00', '2021-01-31 11:00:00+01', 50),
   (3,'Browse','2021-01-31 11:20:00', '2021-01-31 11:20:00-04', 30),
   (3,'Click','2021-01-31 12:00:00', '2021-01-31 12:00:00-04', 80),
   (4,'Browse','2021-01-31 11:50:00', '2021-01-31 11:50:00-01', 1000),
   (4,'Pay','2021-01-31 12:00:00', '2021-01-31 12:00:00-02', 900),
   (4,'Order','2021-01-31 12:00:00', '2021-01-31 12:00:00-03', 1001),
   (4,'Click','2021-01-31 12:00:00', '2021-01-31 12:00:00-04', 1001),
   (5,'Browse','2021-01-31 11:50:00', '2021-01-31 11:50:00+08', NULL),
   (5,'Click','2021-01-31 12:00:00', '2021-01-31 12:00:00+08', 776),
   (5,'Order','2021-01-31 11:10:00', '2021-01-31 11:10:00+08', 999),
   (6,'Browse','2021-01-31 11:50:00', '2021-01-31 11:50:00+01', -1),
   (6,'Click','2021-01-31 12:00:00', '2021-01-31 12:00:00+02', -2),
   (6,'Order','2021-01-31 12:10:00', '2021-01-31 12:00:00+03', -3);

Calculate the funnel for each user. In the **level** column of the following command output, **0** means that there were no events in the window period and **1** means that there was one event in the window period.

.. code-block::

   SELECT
       user_id,
     windowFunnel(
       0, 'default', event_timez,
       event_type = 'Browse', event_type = 'Click', event_type = 'Order', event_type = 'Pay'
     ) AS level
   FROM funnel_test
   GROUP BY user_id
   ORDER by user_id;

    user_id | level
   ---------+-------
          1 |     1
          2 |     0
          3 |     1
          4 |     1
          5 |     1
          6 |     1
   (6 rows)

Calculate the funnel of each user and specify the length of the sliding time window as **NULL**. An error is reported.

.. code-block::

   SELECT
       user_id,
     windowFunnel(
       NULL, 'default', event_time,
       event_type = 'Browse', event_type = 'Click', event_type = 'Order', event_type = 'Pay'
     ) AS level
   FROM funnel_test
   GROUP BY user_id
   ORDER by user_id;
   ERROR:  Invalid parameter : window length or mode is null.

Calculate the funnel of each user and specify multiple conditions.

.. code-block::

   SELECT
       user_id,
     windowFunnel(
       40, 'default', date(event_time),
       true, true, false, true
     ) AS level
   FROM funnel_test
   GROUP BY user_id
   ORDER by user_id;
    user_id | level
   ---------+-------
          1 |     2
          2 |     2
          3 |     2
          4 |     2
          5 |     2
          6 |     2
   (6 rows)

Check the retention rate of each user in each event.

.. code-block::

   SELECT
       user_id,
     retention(
       event_type = 'Browse', event_type = 'Click', event_type = 'Order', event_type = 'Pay'
       ) AS r
   FROM funnel_test
   GROUP BY user_id
   ORDER BY user_id ASC;
    user_id |     r
   ---------+-----------
          1 | {1,1,1,1}
          2 | {0,0,0,0}
          3 | {1,1,0,0}
          4 | {1,1,1,1}
          5 | {1,1,1,0}
          6 | {1,1,1,0}
   (6 rows)

Analyze the retention rate of each user in each event and set the first time point to **false**.

.. code-block::

   SELECT
       user_id,
     retention(
       false, event_type = 'Browse', event_type = 'Click', event_type = 'Order', event_type = 'Pay'
       ) AS r
   FROM funnel_test
   GROUP BY user_id
   ORDER BY user_id ASC;
    user_id |      r
   ---------+-------------
          1 | {0,0,0,0,0}
          2 | {0,0,0,0,0}
          3 | {0,0,0,0,0}
          4 | {0,0,0,0,0}
          5 | {0,0,0,0,0}
          6 | {0,0,0,0,0}
   (6 rows)

Analyze the retention rate of all users in each event.

.. code-block::

   SELECT sum(r[1]), sum(r[2]), sum(r[3]), sum(r[4])
   FROM
   (
       SELECT
       retention(event_type = 'Browse', event_type = 'Click', event_type = 'Order', event_type = 'Pay') AS r
       FROM funnel_test
       GROUP BY user_id
   );
    sum | sum | sum | sum
   -----+-----+-----+-----
      5 |   5 |   4 |   2
   (1 row)

Create the **retention_test** table.

.. code-block::

   CREATE TABLE retention_test(
   uid INT,
   event TEXT,
   event_time TIMESTAMP
   );

Insert data.

::

   INSERT INTO retention_test VALUES
   (1, 'pay', '2024-05-01'),
   (1, 'login', '2024-05-01'),
   (1, 'pay', '2024-05-02'),
   (1, 'login', '2024-05-02'),
   (2, 'login', '2024-05-01'),
   (3, 'login', '2024-05-02'),
   (3, 'pay', '2024-05-03'),
   (3, 'pay', '2024-05-04');

Gather statistics on the retention rate of each user in the payment event after one or two days.

::

   WITH retention_count_info AS (
       SELECT
           uid,
           range_retention_count(event = 'login', event = 'pay',
                                  DATE(event_time), array[1, 2], 'day') AS info
       FROM
           retention_test
       GROUP BY
           uid
   ), retention_sum AS (
            SELECT regexp_split_to_array(unnest(range_retention_sum(info)), ',') AS s
            FROM retention_count_info
        ) SELECT to_date(s[1]::int) AS login_date,
                s[3]::numeric / s[2]::numeric AS retention_rate1,
                s[4]::numeric / s[2]::numeric AS retention_rate2
   FROM retention_sum
   ORDER BY login_date;
        login_date      |    retention_rate1    |    retention_rate2
   ---------------------+-----------------------+------------------------
    2024-05-01 00:00:00 | .50000000000000000000 | 0.00000000000000000000
    2024-05-02 00:00:00 | .50000000000000000000 |  .50000000000000000000
   (2 rows)
