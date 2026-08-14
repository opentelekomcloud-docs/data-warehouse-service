:original_name: dws_04_0740.html

.. _dws_04_0740:

PG_POOLER_STATUS
================

**PG_POOLER_STATUS** displays the cache connection status in the pooler. **PG_POOLER_STATUS** can only query on the CN, and displays the connection cache information about the pooler module.

.. table:: **Table 1** PG_POOLER_STATUS columns

   +-----------------------+-----------------------+----------------------------------------------------+
   | Column                | Type                  | Description                                        |
   +=======================+=======================+====================================================+
   | database              | Text                  | Database name                                      |
   +-----------------------+-----------------------+----------------------------------------------------+
   | user_name             | Text                  | Username                                           |
   +-----------------------+-----------------------+----------------------------------------------------+
   | tid                   | Bigint                | ID of the thread used for the connection to the CN |
   +-----------------------+-----------------------+----------------------------------------------------+
   | node_oid              | Bigint                | OID of the node connected                          |
   +-----------------------+-----------------------+----------------------------------------------------+
   | node_name             | Name                  | Name of the node connected                         |
   +-----------------------+-----------------------+----------------------------------------------------+
   | in_use                | boolean               | Whether the connection is in use. The options are: |
   |                       |                       |                                                    |
   |                       |                       | -  **t** (true): The connection is in use.         |
   |                       |                       | -  **f** (false): The connection is not in use.    |
   +-----------------------+-----------------------+----------------------------------------------------+
   | fdsock                | Bigint                | Peer socket                                        |
   +-----------------------+-----------------------+----------------------------------------------------+
   | remote_pid            | Bigint                | Peer thread ID                                     |
   +-----------------------+-----------------------+----------------------------------------------------+
   | session_params        | Text                  | GUC session parameter delivered by the connection  |
   +-----------------------+-----------------------+----------------------------------------------------+

Example
-------

View information about the connection pool **pooler**:

::

   SELECT database,user_name,node_name,in_use,count(*) FROM pg_pooler_status GROUP BY 1, 2, 3 ,4 ORDER BY 5 DESC LIMIT 50;

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002622089235.png
