:original_name: dws_04_0421.html

.. _dws_04_0421:

.. _en-us_topic_0000001578750678:

Application Scenarios and Restrictions
======================================

Context
-------

The SMP feature improves the performance through operator parallelism and occupies more system resources, including CPU, memory, network, and I/O. Actually, SMP is a method consuming resources to save time. It improves system performance in appropriate scenarios and when resources are sufficient, but may deteriorate performance otherwise. In addition, compared with the serial processing, SMP generates more candidate plans, which is more time-consuming and may deteriorate performance.

Applicable Scenarios
--------------------

-  Operators supporting parallel processing are used.

   The execution plan contains the following operators:

   #. Scan: Row Storage common table and a line memory partition table sequential scanning, column-oriented storage ordinary table and column-oriented storage partition table sequential scanning, HDFS internal and external table sequence scanning. Surface scanning GDS data can be imported at the same time. All of the above does not support replication tables.
   #. Join: HashJoin, NestLoop
   #. Agg: HashAgg, SortAgg, PlainAgg, and WindowAgg, which supports only **partition by**, and does not support **order by**.
   #. Stream: Redistribute, Broadcast
   #. Other: Result, Subqueryscan, Unique, Material, Setop, Append, VectoRow, RowToVec

-  SMP-unique operators

   To execute queries in parallel, Stream operators are added for data exchange of the SMP feature. These new operators can be considered as the subtypes of Stream operators.

   #. Local Gather aggregates data of parallel threads within a DN
   #. Local Redistribute redistributes data based on the distributed key across threads within a DN
   #. Local Broadcast broadcasts data to each thread within a DN.
   #. Local RoundRobin distributes data in polling mode across threads within a DN.
   #. Split Redistribute redistributes data across parallel threads on different DNs.
   #. Split Broadcast broadcasts data to all parallel DN threads in the cluster.

   Among these operators, Local operators exchange data between parallel threads within a DN, and non-Local operators exchange data across DNs.

-  Example

   The TPCH Q1 parallel plan is used as an example.

   |image1|

   In this plan, implement the Hdfs Scan and HashAgg operator parallel, and adds the Local Gather and Split Redistribute data exchange operator.

   In this example, the sixth operator is Split Redistribute, and **dop: 4/4** next to the operator indicates that the degree of parallelism of the sender and receiver is 4. 4 No operator is Local Gather, marked dop: 1/4 above, this operator sender thread parallel degree is 4, while the receiving end thread parallelism degree to 1, that is, lower-layer 5 number Hash Aggregate operators according to the 4 parallel degree, while the working mode of the port on the upper-layer 1 to 3 number operator according to the executed one by one, 4 number operator is used to achieve intra-DN concurrent threads data aggregation.

   You can view the parallelism situation of each operator in the dop information.

Non-applicable Scenarios
------------------------

#. Short query operations are performed, where the plan generation is time-consuming.
#. Operators are processed on CNs.
#. Statements that cannot be pushed down are executed.
#. The **subplan** of a query and operators containing a subquery are executed.

.. |image1| image:: /_static/images/en-us_image_0000001233681849.jpg
