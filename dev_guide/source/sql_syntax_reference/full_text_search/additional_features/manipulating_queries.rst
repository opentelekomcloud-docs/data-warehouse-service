:original_name: dws_06_0098.html

.. _dws_06_0098:

Manipulating Queries
====================

GaussDB(DWS) provides functions and operators that can be used to manipulate queries that are already in tsquery type.

-  tsquery && tsquery

   Returns the AND-combination of the two given tsqueries.

-  tsquery \|\| tsquery

   Returns the OR-combination of the two given tsqueries.

-  !! tsquery

   Returns the negation (NOT) of the given tsquery.

-  numnode(query tsquery) returns integer

   Returns the number of nodes (lexemes plus operators) in a **tsquery**. This function is useful to determine if the query is meaningful (returns > 0), or contains only stop words (returns 0). For example:

   ::

      SELECT numnode(plainto_tsquery('the any'));
      NOTICE:  text-search query contains only stop words or doesn't contain lexemes, ignored
      CONTEXT:  referenced column: numnode
       numnode
      ---------
             0

      SELECT numnode('foo & bar'::tsquery);
       numnode
      ---------
             3

-  querytree(query tsquery) returns text

   Returns the portion of a **tsquery** that can be used for searching an index. This function is useful for detecting unindexable queries, for example those containing only stop words or only negated terms. For example:

   ::

      SELECT querytree(to_tsquery('!defined'));
       querytree
      -----------
       T
      (1 row)
