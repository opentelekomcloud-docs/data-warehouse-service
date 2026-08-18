:original_name: dws_04_1417.html

.. _dws_04_1417:

Overview
========

DWS supports query of materialized views and query rewriting. Materialized view query rewriting allows users to speed up query execution using materialized views automatically, without altering services. This feature lets you add or remove materialized views similar to indexes, without impacting services.

Scenarios
---------

Materialized view query rewriting can avoid complex association, aggregation, and expression calculation operations in the query.

Text matching query rewriting allows queries to match the definition of a materialized view, even if the format and comments differ. It supports complex queries like CTE and window functions. If a complex query performs poorly, creating a materialized view with the same statement can speed up the query.

By default, query rewriting can be performed only when materialized views are valid. You can also set the **force_rewrite_timeout** parameter in the CREATE MATERIALIZED VIEW syntax to specify a period during which a materialized view can still support query writing even if it is invalid.

Query Rewriting Methods
-----------------------

Text matching query rewriting

When the query matches the SQL or syntax tree of the materialized view, you can use the materialized view to rewrite the query.

Setting Materialized View Query Rewriting
-----------------------------------------

The GUC parameter :ref:`mv_rewrite_rule <en-us_topic_0000001764491796__section1683821583115>` of DWS specifies the query rewriting mode.
