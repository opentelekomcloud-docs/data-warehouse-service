:original_name: dws_01_00013.html

.. _dws_01_00013:

Application Scenarios
=====================

Enhanced ETL + Real-time BI analysis
------------------------------------


.. figure:: /_static/images/en-us_image_0000002397118524.png
   :alt: **Figure 1** ETL+BI analysis

   **Figure 1** ETL+BI analysis

The data warehouse is the pillar of the Business Intelligence (BI) system for collecting, storing, and analyzing massive amounts of data. It provides powerful business analysis support for mobile Internet, gaming, and Online to Offline (O2O) industries.

Advantages of DWS are as follows:

-  **Data migration**: efficient and real-time data import in batches from multiple data sources
-  **High performance**: cost-effective PB-level data storage and second-level response to correlation analysis of trillions of data records
-  **Real-time**: real-time consolidation of service data for timely optimization and adjustment of operation decision-making

E-commerce
----------


.. figure:: /_static/images/en-us_image_0000002429538417.png
   :alt: **Figure 2** E-commerce

   **Figure 2** E-commerce

The analyzed data is used for marketing recommendation, operations analysis, full-text search, and customer analysis.

Advantages of DWS are as follows:

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

DWS 3.0 utilizes the HStore storage engine to store micro-batch data locally and syncs it to OBS at regular intervals. It enables high-throughput real-time write and update, as well as large-scale data writes.

Real-time data is written and calculated, and can be used for dashboard statistics, analysis, monitoring, risk control, and recommendations.

|image2|

Service Isolation and Ultimate Elasticity with Multiple Virtual Warehouses (Storage-Compute Decoupling)
-------------------------------------------------------------------------------------------------------

-  Virtual warehouses (VWs) isolate service loads more effectively than soft isolation methods, using VM-level hard isolation to minimize service impact.

-  Multiple classic VWs and multiple elastic VWs are supported.

   |image3|

-  Classic VWs are used to isolate services.

   -  VWs can be deployed based on service needs, with different services bound to different VWs. Classic VWs allow table creation.
   -  Resources are isolated between VWs so that services do not affect each other.
   -  Data is shared between VWs in real time.
   -  The performance ceiling for a single SQL statement within our MPP architecture is determined by the size of a fixed VW.
   -  Fixed VWs are optimized for consistent workloads and low-latency operations, such as real-time data access and processing. The size of fixed VWs can be proactively planned to accommodate anticipated service fluctuations.

-  Concurrent expansion through elastic VW

   -  In high-concurrency scenarios, elastic VWs are dynamically created to handle queued services. These VWs support read and write operations, but not table creation.
   -  Elastic VWs automatically handle queuing queries.
   -  Elastic VWs seamlessly absorbs queued queries to enhance service concurrency.
   -  As demand subsides, elastic VWs are automatically decommissioned.
   -  Elastic VWs offer on-demand resource allocation, with the flexibility for users to define upper limits.
   -  Despite their dynamic nature, elastic VWs maintain the same specifications as fixed VWs, ensuring consistent SQL statement performance.
   -  Elastic VWs are suitable for handling sporadic and cyclical workloads.

For example, if a customer has multiple service departments, each can be assigned a classic VW to isolate resources. If Service 1 uses a three-node VW and Service 2 uses a four-node VW, and Service 1 has peak hours from 10:00 to 12:00, elastic VWs can be configured to scale during peak hours and be destroyed afterward.

.. |image1| image:: /_static/images/en-us_image_0000002270374377.png
.. |image2| image:: /_static/images/en-us_image_0000002270494329.png
.. |image3| image:: /_static/images/en-us_image_0000002270494325.png
