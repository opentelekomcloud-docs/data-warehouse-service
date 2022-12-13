:original_name: dws_03_2121.html

.. _dws_03_2121:

What Is a Database/Data Warehouse/Data Lake/Lakehouse?
======================================================

The evolving Internet and IoT produce massive volumes of data. This data needs to be managed, using concepts like database, data warehouse, data lake, and lakehouse. Here is an overview of how they map with our products and solutions.

Database
--------

A database is where data is organized, stored, and managed by data structure.

Databases have been used in computers since the 1960s, with the two prevailing data models (hierarchical and network), and data and applications were very interdependent. This limited database applications.

A database usually refers to a relational database. A relational database organizes data with a relational model, that is, data is stored in rows and columns. Therefore, database data is well-structured and independent with low redundancy. In 1970, relational databases were born to completely separate data from applications for software and have become an indispensable part of mainstream computer systems. Relational databases are the foundation of database products from all vendors, with relational API support even if a database is non-relational.

Relational databases process basic and routine transactions using OLTP, such as bank transactions.

Data Warehouse
--------------

Database growth has facilitated data growth. OLAP explores the relationship between data and mines more data value. However, it is difficult to share data between different databases, and data integration and analysis also face great challenges.

To overcome these challenges for enterprises, Bill Inmon, proposed the idea of data warehousing in 1990. The data warehouse runs on a unique storage architecture to perform OLAP on a large amount of the OLTP data accumulated over the years. In this way, enterprises can obtain valuable information from massive data quickly and effectively to make informed decisions. Thanks to data warehouses, the information industry has evolved from operational systems based on relational databases to decision support systems.

Unlike a database, a data warehouse has the following features:

-  A data warehouse uses themes. It is built to support various services, with data coming from scattered operational data. Therefore, the required data needs to be extracted from multiple heterogeneous data sources, processed and integrated, and reorganized by theme.
-  A data warehouse mainly supports enterprise decision analysis and the operations involved are focused on data query. Therefore, it improves the query speed and cuts the total cost of ownership (TCO) by optimizing table structures and storage modes.

.. table:: **Table 1** Comparison between data warehouses and databases

   +----------------------+---------------------------+----------------------------------+
   | Dimension            | Data Warehouse            | Database                         |
   +======================+===========================+==================================+
   | Application scenario | OLAP                      | OLTP                             |
   +----------------------+---------------------------+----------------------------------+
   | Data source          | Multiple                  | Single                           |
   +----------------------+---------------------------+----------------------------------+
   | Data normalization   | Denormalized schemas      | Highly normalized static schemas |
   +----------------------+---------------------------+----------------------------------+
   | Data access          | Optimized read operations | Optimized write operations       |
   +----------------------+---------------------------+----------------------------------+

Data Lake
---------

Data is an important asset for enterprises. Production and operations data are saved and distilled into effective management policies.

The data lake does that. It is a large data warehouse that centrally stores structured and unstructured data. It can store raw data of multiple data sources and types, meaning that data can be accessed, processed, analyzed, and transmitted without being structured first. The data lake helps enterprises quickly complete federated analysis of heterogeneous data sources and explore data value.

A data lake is in essence a solution that consists of a data storage architecture and data processing tools.

-  The **storage architecture** must be scalable and reliable enough to store massive data of any type (structured, semi-structured, unstructured data).
-  The two types of **processing tools** have separate functions:

   -  The first type: migrates data into the lake, including defining sources, formulating synchronization policies, moving data, and compiling catalogs.
   -  The second type then uses that data, including analyzing, mining, and using it. The data lake must be equipped with wide-ranging capabilities, such as comprehensive data and data lifecycle management, diversified data analytics, and secure data acquisition and release. These data governance tools help guarantee data quality, which can be compromised by a lack of metadata and turn the data lake into a data swamp.

Now with big data and AI, lake data is even more valuable and plays new roles. It represents more enterprise capabilities. For example, the data lake can centralize data management, helping enterprises build more optimized operation models. It also provides other enterprise capabilities such as prediction analysis and recommendation models. These models can stimulate further growth.

Just like any other warehouse and lake, one stores goods, or data, from one source while the other stores water, or data, from many sources.

.. table:: **Table 2** Comparison between data lakes and data warehouses

   +----------------------+---------------------------------------------------------------------------------+----------------------------------------------------------+
   | Dimension            | Data Lake                                                                       | Data Warehouse                                           |
   +======================+=================================================================================+==========================================================+
   | Application scenario | Exploratory analytics (machine learning, data discovery, profiling, prediction) | Data analytics (based on historical structured data)     |
   +----------------------+---------------------------------------------------------------------------------+----------------------------------------------------------+
   | Cost                 | Low initial cost, high subsequent cost                                          | High initial cost, low subsequent cost                   |
   +----------------------+---------------------------------------------------------------------------------+----------------------------------------------------------+
   | Data quality         | Massive raw data to be cleaned and normalized before use                        | High quality data that can be used as the basis of facts |
   +----------------------+---------------------------------------------------------------------------------+----------------------------------------------------------+
   | Target user          | Data scientists and data developers                                             | Business analysts                                        |
   +----------------------+---------------------------------------------------------------------------------+----------------------------------------------------------+

Lakehouse
---------

Although the application scenarios and architectures of a data warehouse and a data lake are different, they can cooperate to resolve problems. A data warehouse stores structured data and is ideal for quick BI and decision-making support, while a data lake stores data in any format and can generate larger value by mining data. Therefore, their convergence can bring more benefits to enterprises in some scenarios.

A lakehouse, the convergence of a data warehouse and a data lake, aims to enable data mobility and streamline construction. The key of the lakehouse architecture is to enable the free flow of data/metadata between the data warehouse and the data lake. The explicit-value data in the lake can flow to or even be directly used by the warehouse. The implicit-value data in the warehouse can also flow to the lake for long-term storage at a low cost and for future data mining.

Data Enablement Solution
------------------------

Data Lake Governance Center (DGC) is a data enablement platform that helps large government agencies and companies customize intelligent data resource management solutions. This solution can import all-domain data into the data lake, eliminating data silos, unleashing the value of data, and empowering data-driven digital transformation.

DGC features the FusionInsight intelligent data lake as its core. Around it are computing engines such as the database, data warehouse, data lake, and data platform. DGC provides comprehensive data enablement, covering data collection, aggregation, computing, asset management, and data openness.

Lake, warehouse, and database engines enable agile data lake construction, fast migration of GaussDB databases, and real-time analysis of the data warehouse. For more information, go to:

-  Database

   -  Relational database: Relational Database Service (RDS), GaussDB(for MySQL), GaussDB(for openGauss), RDS for PostgreSQL, RDS for SQL Server
   -  Non-relational database: Document Database Service (DDS), GaussDB NoSQL (including Influx, Redis, Mongo, Cassandra)

-  Data warehouse: GaussDB(DWS)
-  Data lake or lakehouse: MapReduce Service (MRS), Data Lake Insight (DLI)
-  Data governance center: Data Lake Governance Center (DGC)
