:original_name: dws_06_0232.html

.. _dws_06_0232:

EXPLAIN
=======

Function
--------

Shows the execution plan of an SQL statement.

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

-  Display the execution plan of an SQL statement, which supports multiple options in no specific order.

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
          FORMAT { TEXT | XML | JSON | YAML }

-  Display the execution plan of an SQL statement, where options are in order.

   ::

      EXPLAIN  { [  { ANALYZE  | ANALYSE  }  ] [ VERBOSE  ]  | PERFORMANCE  } statement;

-  Display information required for reproducing the execution plan of an SQL statement. The information is usually used for fault locating. The **STATS** option must be used independently.

   ::

      EXPLAIN ( STATS [ boolean ] ) statement;

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

-  **PERFORMANCE**

   This option prints all relevant information in execution.

-  **STATS boolean**

   Specifies whether to display information required for reproducing the execution plan of an SQL statement, including the object definition, statistics, and configuration parameters. The information is usually used for fault locating.

   Valid value:

   -  **TRUE** (default value): Display information required for reproducing the execution plan of an SQL statement.
   -  **FALSE**: No display.

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

Change the value of **explain_perf_mode** to **pretty**.

::

   SET explain_perf_mode=pretty;

Helpful Links
-------------

:ref:`ANALYZE | ANALYSE <dws_06_0245>`
