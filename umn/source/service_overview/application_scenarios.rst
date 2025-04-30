:original_name: dws_01_00013.html

.. _dws_01_00013:

Application Scenarios
=====================

Enhanced ETL + Real-time BI analysis
------------------------------------


.. figure:: /_static/images/en-us_image_0000002203312685.png
   :alt: **Figure 1** ETL+BI analysis

   **Figure 1** ETL+BI analysis

The data warehouse is the pillar of the Business Intelligence (BI) system for collecting, storing, and analyzing massive amounts of data. It provides powerful business analysis support for mobile Internet, gaming, and Online to Offline (O2O) industries.

Advantages of GaussDB(DWS) are as follows:

-  **Data migration**: efficient and real-time data import in batches from multiple data sources
-  **High performance**: cost-effective PB-level data storage and second-level response to correlation analysis of trillions of data records
-  **Real-time**: real-time consolidation of service data for timely optimization and adjustment of operation decision-making

E-commerce
----------


.. figure:: /_static/images/en-us_image_0000002203312701.png
   :alt: **Figure 2** E-commerce

   **Figure 2** E-commerce

The analyzed data is used for marketing recommendation, operations analysis, full-text search, and customer analysis.

Advantages of GaussDB(DWS) are as follows:

-  **Multi-dimensional analysis**: analysis from products, users, operation, regions, and more
-  **Scale-out as the business grows**: on-demand cluster scale-out as the business grows
-  **High reliability**: long-term stable running of the e-commerce system

Lakehouse
---------

-  **Seamless access to the data lake**

   -  With the interconnection with Hive Metastore metadata management, you can directly access the data table definitions in the data lake. You do not need to create a foreign table. You only need to create an external schema.
   -  The following data formats are supported: ORC and Parquet.

-  **Convergent query**

   -  Hybrid query of any data in the data lake and warehouse is supported.
   -  The query result is directly sent to the warehouse or data lake. No data needs to be transferred or copied.

-  **Excellent query performance**

   -  High-quality query plans and efficient execution engines
   -  Precise load management methods

   |image1|

Real-Time Write
---------------

DWS 3.0 utilizes the H-Store storage engine to store micro-batch data locally and syncs it to OBS at regular intervals. It enables high-throughput real-time write and update, as well as large-scale data writes.

Real-time data is written and calculated, and can be used for dashboard statistics, analysis, monitoring, risk control, and recommendations.

|image2|

.. |image1| image:: /_static/images/en-us_image_0000002203427145.png
.. |image2| image:: /_static/images/en-us_image_0000002203427165.png
