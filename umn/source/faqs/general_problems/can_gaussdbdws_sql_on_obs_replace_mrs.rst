:original_name: dws_03_0011.html

.. _dws_03_0011:

Can GaussDB(DWS) SQL on OBS Replace MRS?
========================================

No. Although SQL on OBS of GaussDB(DWS) can be used for GaussDB(DWS) and OBS data query, it cannot replace the processing frameworks of MRS.

Apart from SQL query, MRS also provides other functions. Public cloud MRS is a hosting service. It helps you use the latest common big data processing frameworks, such as Spark, Hadoop, and HBase, to process and analyze big data sets on a customizable cluster. With public cloud MRS, you can run a wide variety of scale-out data processing tasks for applications such as machine learning, graph analytics, data transformation, and stream data. GaussDB(DWS) SQL on OBS works with MRS. If you have already use MRS to process large-size data storage, GaussDB(DWS) SQL on OBS can be used to query these data without affecting MRS tasks.

In this case, the query service, data warehouses, and complex data processing frameworks play their own roles - simply select the appropriate tool for the task.
