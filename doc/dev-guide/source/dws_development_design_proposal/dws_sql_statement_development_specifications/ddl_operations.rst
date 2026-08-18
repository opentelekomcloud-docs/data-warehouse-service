:original_name: dws_04_0115.html

.. _dws_04_0115:

DDL Operations
==============

.. _en-us_topic_0000002100746054__en-us_topic_0000002100047742_section238722717463:

Suggestion 3.1: Avoiding Performing DDL Operations (Except CREATE) During Peak Hours or in Long Transactions
------------------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   DDL operations like **ALTER**, **DROP**, **TRUNCATE**, **REINDEX**, and **VACUUM FULL** have high lock levels and can block services during execution.

   -  During peak hours, these DDL operations with high lock levels should be avoided to prevent service blockage.
   -  Long transactions involving DDL operations with held or waited locks can also block services.

   **Solution:**

   -  Choose off-peak hours or maintenance windows for DDL operations based on service periods. Specify the DDL execution environment and time consumption to avoid service blockage due to long lock waiting duration.

.. _en-us_topic_0000002100746054__en-us_topic_0000002100047742_section13238718155015:

Rule 3.2: Specifying the Scope of Objects to Be Deleted When Using DROP
-----------------------------------------------------------------------

|image1|

**Impact of rule violation:**

Be cautious when using **DROP OBJECT** (e.g., **DATABASE**, **USER/ROLE**, **SCHEMA**, **TABLE**, **VIEW**) as it may cause data loss, especially with **CASCADE** deletions.

-  **DROP DATABASE**: deletes all objects in the database.
-  **DROP USER**: deletes the **USER** object and its schemas and table objects.
-  **DROP SCHEMA**: deletes all objects in the schema.
-  **DROP TABLE**: deletes the **TABLE** object and the indexes and views that depend on it.

**Solution:**

-  Exercise caution when performing the **DROP** operation and back up data in advance.

.. |image1| image:: /_static/images/danger_3.0-en-us.png
