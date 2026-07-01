:original_name: dws_01_0802.html

.. _dws_01_0802:

Overview
========

DWS provides you with a one-stop data development tool, that is, the online SQL editor, for data development, access, and processing.

The data development tool lets you connect directly to the cluster database on the DWS console. It shows details about the database's metadata and lets you edit and run SQL statements. Results appear in charts and tables. You can save SQL statements to OBS as text files.

-  This tool is supported only by clusters of version 8.1.3 or later.
-  This tool is not included in the upgrade of the management plane to version 8.3.1 or later. The SQL editor is available only when the management plane of version 8.3.1 or later is installed and deployed for the first time.

-  The editor depends on DWS and OBS. You need to enable the DWS cluster query and OBS query operations, and interconnect the editor with the Cloud Trace Service (CTS) service to record traces of operation APIs.

Editor Functions
----------------

-  In the upper part of the editor, you can switch the data source, database, and schema.
-  SQL statements can be written in the middle, where highlighting, basic syntax tips, and information about databases, schemas, tables, and fields are provided.
-  You can format SQL statements and query execution plans with this tool. But be careful, the PERFORMANCE execution plan executes SQL statements. So, avoid using it for SQL statements that perform operations.
-  You can click **Save** after writing many SQL statements. Then a dialog box will ask you to save them to the right OBS bucket.
-  The query results show at the bottom in multiple pages. You can make pie, line, or bar charts from different fields. You can also export the results to an Excel file. The **SQL execution records** area displays non-query SQL statement records from the past six months.
-  You can switch to the script panel and show the directory folder. The script file is saved in the created directory. For details, see :ref:`Creating a Directory <en-us_topic_0000002235334760__section383484418124>`. The editor allows for two directory levels, each with a capacity of 10 folders. Each folder can hold up to 100 script files, which are stored in the corresponding OBS bucket file directory. To make things easier, the OBS bucket file address can be set globally. For details, see :ref:`Global Settings <en-us_topic_0000002270373901__section129025518555>`.
