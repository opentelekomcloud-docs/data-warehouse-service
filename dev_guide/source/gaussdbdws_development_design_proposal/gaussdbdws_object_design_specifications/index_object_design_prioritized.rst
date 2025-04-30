:original_name: dws_04_0113.html

.. _dws_04_0113:

INDEX Object Design (Prioritized)
=================================

.. _en-us_topic_0000002136265441__en-us_topic_0000002100207558_section13748110124413:

Rule 2.14: Creating Necessary Indexes and Selecting Optimal Columns and Sequences for Them
------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Redundant indexes consume unnecessary space and can impact data import efficiency.

   -  The column sequence in the composite index is incorrect, affecting the query efficiency.

   **Best practices:**

   The following conditions must be met when indexes are used:

   -  The index column should be a column commonly used for filtering or joining conditions.

   -  The index column should have more distinct values.
   -  When creating a multi-column combination index, prioritize columns with more distinct values.
   -  The number of indexes in a single table should be limited to less than five. You can control the number of indexes by combining them.
   -  In scenarios where data is added, deleted, or modified in batches, delete the index first and then add it back after the batch operation is complete to improve performance (real-time access may be affected).

.. _en-us_topic_0000002136265441__en-us_topic_0000002100207558_section114813162441:

Suggestion 2.15: Optimizing Performance by Choosing the Right Index Type and Avoiding Indexes for Column-Store Tables
---------------------------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Incorrect indexes do not improve column-store access and can negatively affect query performance.

   **Solution:**

   #. Specify the appropriate index type when creating indexes, avoiding the default psort index.
   #. In point queries where small amounts of data need to be retrieved from mass datasets, consider using a B-tree index.
   #. For high range query performance, create a partial cluster key (PCK) to quickly filter and scan fact tables using the min/max sparse index. Comply with the following rules to create a PCK:

      -  [Notice] Only one PCK can be created in a table. A PCK can contain multiple columns, preferably no more than two columns.
      -  [Suggestion] Create a PCK for the filter condition column of the expression (e.g., **col op const**, where **op** is the operator =, >, >=, <=, and <, and **const** is a constant value).
