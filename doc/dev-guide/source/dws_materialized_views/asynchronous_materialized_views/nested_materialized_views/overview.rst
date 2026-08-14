:original_name: dws_04_1426.html

.. _dws_04_1426:

Overview
========

DWS materialized views support nested creation, refreshing, and rewriting. Using nested materialized views can simplify the process of defining a single view.

-  Creating a nested materialized view: You can build a materialized view based on the materialized view.
-  Cascading refresh: When refreshing a nested materialized view, not only the current view is refreshed, but automatic cascading refresh is also supported.
-  Scheduled cascading refresh: You can specify and modify the refresh mode for periodically refreshed materialized views.
-  Rewriting nested materialized views: Query statements can be automatically rewritten as SQL statements for querying materialized views.
