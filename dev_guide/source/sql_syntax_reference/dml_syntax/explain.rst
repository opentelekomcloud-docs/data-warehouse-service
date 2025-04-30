:original_name: dws_06_0232.html

.. _dws_06_0232:

EXPLAIN
=======

Function
--------

**EXPLAIN** shows the execution plan of an SQL statement.

The execution plan shows how the tables referenced by the SQL statement will be scanned, for example, by plain sequential scan or index scan. If multiple tables are referenced, the execution plan also shows what join algorithms will be used to bring together the required rows from each input table.

The most critical part of the display is the estimated statement execution cost, which is the planner's guess at how long it will take to run the statement.

The **ANALYZE** option causes the statement to be executed, not only planned. Then actual runtime statistics are added to the display, including the total elapsed time expended within each plan node (in milliseconds) and the total number of rows it actually returned. This is useful to check whether the planner's estimates are close to reality.

Precautions
-----------

The statement is executed when the **ANALYZE** option is used. To use **EXPLAIN ANALYZE** on an **INSERT**, **UPDATE**, **DELETE**, **CREATE TABLE AS**, or **EXECUTE** statement without letting the command affect your data, use this approach:

::

   START TRANSACTION;
   EXPLAIN ANALYZE ...;
   ROLLBACK;

Syntax
------

-  Display the execution plan of an SQL statement, which supports multiple options and has no requirements for the order of options.

   ::

      EXPLAIN [ (  option  [, ...] )  ] statement;

   The syntax of the **option** clause is as follows:

   ::

      ANALYZE [ boolean ] |
          ANALYSE [ boolean ] |
          VERBOSE [ boolean ] |
          COSTS [ boolean ] |
          CPU [ boolean ] |
          DETAIL [ boolean ] |
          NODES [ boolean ] |
          NUM_NODES [ boolean ] |
          BUFFERS [ boolean ] |
          TIMING [ boolean ] |
          PLAN [ boolean ] |
          FORMAT { TEXT | XML | JSON | YAML } |
          BLOCKNAME [boolean] |
          OUTLINE [boolean]  |
          WARMUP |
          WARMUP HOT

-  Display the execution plan of an SQL statement, where options are in order.

   ::

      EXPLAIN  { [  { ANALYZE  | ANALYSE  }  ] [ VERBOSE  ]  | PERFORMANCE  } statement;

-  Display information required for reproducing the execution plan of an SQL statement. The information is usually used for fault locating. The **STATS** option must be used independently.

   ::

      EXPLAIN ( STATS [ boolean ] ) statement;

-  Display the detailed time consumption of the DDL statement execution. This syntax is supported only by clusters of version 9.1.0 or later.

   ::

      EXPLAIN PERFORMANCE statement;

   DDL statement such as CREATE, CREATE INDEX, DROP, VACUUM FULL, ANALYZE, and COPY are supported.

-  Perform pre-query and cache the pre-queried data to the local disk for higher query speed. This syntax is supported only by clusters of 9.1.0.200 and later versions.

   ::

      EXPLAIN WARMUP statement;

   ::

      EXPLAIN WARMUP HOT statement;

Parameter Description
---------------------

-  **statement**

   Specifies the SQL statement to explain.

-  **ANALYZE boolean \| ANALYSE boolean**

   Displays the actual run times and other statistics.

   Valid value:

   -  **TRUE** (default value): Displays the actual run times and other statistics.
   -  **FALSE**: No display.

-  **VERBOSE boolean**

   Displays additional information regarding the plan.

   Valid value:

   -  **TRUE** (default value): Displays additional information.
   -  **FALSE**: No display.

-  **COSTS boolean**

   Includes information on the estimated total cost of each plan node, as well as the estimated number of rows and the estimated width of each row.

   Valid value:

   -  **TRUE** (default): Displays information on the estimated total cost of each plan node and the estimated width of each row.
   -  **FALSE**: No display.

-  **CPU boolean**

   Prints information on CPU usage.

   Valid value:

   -  **TRUE** (default value): Displays CPU usage information.
   -  **FALSE**: No display.

-  **DETAIL boolean**

   Prints DN information.

   Valid value:

   -  **TRUE** (default value): Prints DN information.
   -  **FALSE**: No display.

   .. note::

      In 8.2.1 and later cluster versions, if **Detail** is enabled in **EXPLAIN**, the skew value comparison time is displayed in the execution plan.

-  **NODES boolean**

   Prints information about the nodes executed by query.

   Valid value:

   -  **TRUE** (default): Prints information about executed nodes.
   -  **FALSE**: No display.

-  **NUM_NODES boolean**

   Prints the quantity of executing nodes.

   Valid value:

   -  **TRUE** (default value): Prints the number of DNs.
   -  **FALSE**: No display.

-  **BUFFERS boolean**

   Includes information on buffer usage.

   Valid value:

   -  **TRUE**: Displays information on buffer usage.
   -  **FALSE** (default): No display.

-  **TIMING boolean**

   Includes the startup time and the time spent on the output node.

   Valid value:

   -  **TRUE** (Default): Displays the startup time and the time spent on the output node.
   -  **FALSE**: No display.

-  **PLAN**

   Specifies whether to store the execution plan in **PLAN_TABLE**. If this parameter is set to **on**, the execution plan is stored in **PLAN_TABLE** and is not displayed on the screen. Therefore, this parameter cannot be used together with other parameters when it is set to **on**.

   Valid value:

   -  **on**: The execution plan is stored in **PLAN_TABLE** and is not printed on the screen. It is the default value. If the plan is stored successfully, **EXPLAIN SUCCESS** is returned.
   -  **off**: The execution plan is not stored in **PLAN_TABLE** and is printed on the screen.

-  **FORMAT**

   Specifies the output format.

   Value range: **TEXT**, **XML**, **JSON**, and **YAML**.

   Default value: **TEXT**

-  **BLOCKNAME boolean**

   Displays the blockname information of an operator.

   .. note::

      The blockname information is printed only when **explain_perf_mode** is set to **pretty**.

   **Value range**: Boolean

   -  **TRUE**: displays blockname information.
   -  **FALSE**: displays no blockname information.

   **Default value**: TRUE

-  **OUTLINE boolean**

   Displays outline information extracted from the plan.

   .. note::

      The outline is printed only when **explain_perf_mode** is set to **pretty**.

   **Value range**: Boolean

   -  **TRUE**: displays outline information.
   -  **FALSE**: displays no outline information.

   **Default value**: TRUE

-  **PERFORMANCE**

   This option prints all relevant information in execution.

-  **STATS boolean**

   Specifies whether to display information required for reproducing the execution plan of an SQL statement, including the object definition, statistics, and configuration parameters. The information is usually used for fault locating.

   Valid value:

   -  **TRUE** (default value): Display information required for reproducing the execution plan of an SQL statement.
   -  **FALSE**: No display.

-  **WARMUP**

   Processes the queried data in the sequence of A1in > A1out > Am. This is supported only by clusters of version 9.1.0.200 or later.

-  **WARMUP HOT**

   Directly adds the queried data to the Am column. This is supported only by clusters of version 9.1.0.200 or later.

Examples
--------

Create the **tpcds.customer_address_p1** table.

::

   CREATE TABLE tpcds.customer_address_p1 AS TABLE tpcds.customer_address;

Change the value of **explain_perf_mode** to **normal**.

::

   SET explain_perf_mode=normal;

Display an execution plan for simple queries in the table.

::

   EXPLAIN SELECT * FROM tpcds.customer_address_p1;
                      QUERY PLAN
   ----------------------------------------------------------------------------
    Data Node Scan on "__REMOTE_FQS_QUERY__"  (cost=0.00..0.00 rows=0 width=0)
      Node/s: All datanodes
   (2 rows)

Generate an execution plan in JSON format (assume **explain_perf_mode** is set to **normal**).

::

   EXPLAIN(FORMAT JSON) SELECT * FROM tpcds.customer_address_p1;
                       QUERY PLAN
   ---------------------------------------------------
    [                                                +
      {                                              +
        "Plan": {                                    +
          "Node Type": "Data Node Scan",             +
          "RemoteQuery name": "__REMOTE_FQS_QUERY__",+
          "Alias": "__REMOTE_FQS_QUERY__",           +
          "Startup Cost": 0.00,                      +
          "Total Cost": 0.00,                        +
          "Plan Rows": 0,                            +
          "Plan Width": 0,                           +
          "Nodes": "All datanodes"                   +
        }                                            +
      }                                              +
    ]
   (1 row)

If there is an index and we use a query with an indexable **WHERE** condition, **EXPLAIN** might show a different plan.

::

   EXPLAIN SELECT * FROM tpcds.customer_address_p1 WHERE ca_address_sk=10000;
                                     QUERY PLAN
   ------------------------------------------------------------------------------
    Data Node Scan on "__REMOTE_LIGHT_QUERY__"  (cost=0.00..0.00 rows=0 width=0)
      Node/s: datanode2
   (2 rows)

Generate an execution plan in YAML format (assume **explain_perf_mode** is set to **normal**).

::

   EXPLAIN(FORMAT YAML) SELECT * FROM tpcds.customer_address_p1 WHERE ca_address_sk=10000;
                      QUERY PLAN
   ------------------------------------------------
    - Plan:                                       +
        Node Type: "Data Node Scan"               +
        RemoteQuery name: "__REMOTE_LIGHT_QUERY__"+
        Alias: "__REMOTE_LIGHT_QUERY__"           +
        Startup Cost: 0.00                        +
        Total Cost: 0.00                          +
        Plan Rows: 0                              +
        Plan Width: 0                             +
        Nodes: "datanode2"
   (1 row)

Here is an example of an execution plan with cost estimates suppressed.

::

   EXPLAIN(COSTS FALSE)SELECT * FROM tpcds.customer_address_p1 WHERE ca_address_sk=10000;
                    QUERY PLAN
   --------------------------------------------
    Data Node Scan on "__REMOTE_LIGHT_QUERY__"
      Node/s: datanode2
   (2 rows)

Here is an example of an execution plan for a query that uses an aggregate function.

::

   EXPLAIN SELECT SUM(ca_address_sk) FROM tpcds.customer_address_p1 WHERE ca_address_sk<10000;
                                         QUERY PLAN
   ---------------------------------------------------------------------------------------
    Aggregate  (cost=18.19..14.32 rows=1 width=4)
      ->  Streaming (type: GATHER)  (cost=18.19..14.32 rows=3 width=4)
            Node/s: All datanodes
            ->  Aggregate  (cost=14.19..14.20 rows=3 width=4)
                  ->  Seq Scan on customer_address_p1  (cost=0.00..14.18 rows=10 width=4)
                        Filter: (ca_address_sk < 10000)
   (6 rows)

Delete the **tpcds.customer_address_p1** table.

::

   DROP TABLE tpcds.customer_address_p1;

Run the **EXPLAIN PERFORMANCE** command for the **ANALYZE** statement.

::

   EXPLAIN PERFORMANCE ANALYZE t2_dist_row;
                               QUERY EXEC INFO
   -----------------------------------------------------------------------
    lock FirstCN:
           coordinator1: actual time=0.240 loops=1
    estimate rows: actual time=[datanode3 0.000, datanode1 0.001]
           coordinator1: actual time=0.000 loops=1
           datanode1: actual time=0.001 loops=1
           datanode2: actual time=0.000 loops=1
           datanode3: actual time=0.000 loops=1
    sample rows: actual time=[datanode1 5.109, coordinator1 119.838]
           coordinator1: actual time=119.838 loops=1
           datanode1: actual time=5.109 loops=1
           datanode2: actual time=5.621 loops=1
           datanode3: actual time=5.342 loops=1
    fetch global stats:
           coordinator1: actual time=8.501 loops=1
    calc stats: actual time=[datanode3 80.794, datanode2 109.155]
           coordinator1: actual time=97.452 loops=1
           datanode1: actual time=94.375 loops=1
           datanode2: actual time=109.155 loops=1
           datanode3: actual time=80.794 loops=1
    calc column stats: actual time=[datanode2 0.938, datanode3 9.811]
           coordinator1: actual time=5.162 loops=2
           datanode1: actual time=1.453 loops=2
           datanode2: actual time=0.938 loops=2
           datanode3: actual time=9.811 loops=2
    calc index stats: actual time=[datanode3 12.392, coordinator1 36.113]
           coordinator1: actual time=36.113 loops=1
           datanode1: actual time=15.933 loops=1
           datanode2: actual time=13.419 loops=1
           datanode3: actual time=12.392 loops=1
    calc expr stats: actual time=[datanode3 41.665, datanode2 78.442]
           coordinator1: actual time=55.608 loops=1
           datanode1: actual time=63.179 loops=1
           datanode2: actual time=78.442 loops=1
           datanode3: actual time=41.665 loops=1
    sync stats:
           coordinator1: actual time=7.906 loops=1

    General Tracks
    CN build CN connection:
           coordinator1: actual time=0.002 loops=1
    CN build DN connection:
           coordinator1: actual time=0.070 loops=1
    -> execute ddl on other CN:
           coordinator1: actual time=0.001 loops=1
    -> execute ddl on other DN:
           coordinator1: actual time=0.000 loops=1
    Query Id: 72902018968225366
    Total runtime: 242.211 ms
   (48 rows)

Show the blockname of a plan.

::

   EXPLAIN (BLOCKNAME ON) SELECT SUM(ca_address_sk) FROM tpcds.customer_address_p1 WHERE ca_address_sk<10000;
                                            QUERY PLAN
   ---------------------------------------------------------------------------------------------
     id |                  operation                   | E-rows | E-memory | E-width | E-costs
    ----+----------------------------------------------+--------+----------+---------+---------
      1 | ->  Aggregate                                |      1 |          |      12 | 16.14
      2 |    ->  Streaming (type: GATHER)              |      2 |          |      12 | 16.14
      3 |       ->  Aggregate                          |      2 | 1MB      |      12 | 10.14
      4 |          ->  Seq Scan on customer_address_p1 |      7 | 1MB      |       4 | 10.12

    Predicate Information (identified by plan id)
    ---------------------------------------------
      4 --Seq Scan on customer_address_p1
            Filter: (ca_address_sk < 10000)

    Query Block Name / Object Alias (identified by plan id)
    -------------------------------------------------------
      1 - sel$1
      4 - sel$1 / customer_address_p1@"sel$1"

      ====== Query Summary =====
    -------------------------------
    System available mem: 4710400KB
    Query Max mem: 4710400KB
    Query estimated mem: 2048KB
   (22 rows)

Show the outline of a plan.

::

   EXPLAIN (OUTLINE ON) SELECT SUM(ca_address_sk) FROM tpcds.customer_address_p1 WHERE ca_address_sk<10000;
                                            QUERY PLAN
   ---------------------------------------------------------------------------------------------
     id |                  operation                   | E-rows | E-memory | E-width | E-costs
    ----+----------------------------------------------+--------+----------+---------+---------
      1 | ->  Aggregate                                |      1 |          |      12 | 16.14
      2 |    ->  Streaming (type: GATHER)              |      2 |          |      12 | 16.14
      3 |       ->  Aggregate                          |      2 | 1MB      |      12 | 10.14
      4 |          ->  Seq Scan on customer_address_p1 |      7 | 1MB      |       4 | 10.12

    Predicate Information (identified by plan id)
    ---------------------------------------------
      4 --Seq Scan on customer_address_p1
            Filter: (ca_address_sk < 10000)

                            Outline Data
    ------------------------------------------------------------
      /*+
          begin_outline_data
           TableScan(@"sel$1" tpcds.customer_address_p1@"sel$1")
          end_outline_data
      */

      ====== Query Summary =====
    -------------------------------
    System available mem: 4710400KB
    Query Max mem: 4710400KB
    Query estimated mem: 2048KB
   (25 rows)

Helpful Links
-------------

:ref:`ANALYZE | ANALYSE <dws_06_0245>`
