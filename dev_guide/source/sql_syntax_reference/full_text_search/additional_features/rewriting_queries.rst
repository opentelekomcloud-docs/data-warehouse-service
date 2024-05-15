:original_name: dws_06_0099.html

.. _dws_06_0099:

Rewriting Queries
=================

The **ts_rewrite** function searches a given **tsquery** for occurrences of a target subquery, and replace each occurrence with a substitute subquery. In essence this operation is a **tsquery** specific version of substring replacement. A target and substitute combination can be thought of as a query rewrite rule. A collection of such rewrite rules can be a powerful search aid. For example, you can expand the search using synonyms (that is, **new york**, **big apple**, **nyc**, **gotham**) or narrow the search to direct the user to some hot topic.

-  ts_rewrite (query tsquery, target tsquery, substitute tsquery) returns tsquery

   This form of **ts_rewrite** simply applies a single rewrite rule: **target** is replaced by **substitute** wherever it appears in query. For example:

   ::

      SELECT ts_rewrite('a & b'::tsquery, 'a'::tsquery, 'c'::tsquery);
       ts_rewrite
      ------------
       'b' & 'c'
      (1 row)

-  ts_rewrite (query tsquery, select text) returns tsquery

   This form of **ts_rewrite** accepts a starting query and a SQL select command, which is given as a text string. The **select** must yield two columns of **tsquery** type. For each row of the select result, occurrences of the first column value (the target) are replaced by the second column value (the substitute) within the current **query** value.

   .. note::

      Note that when multiple rewrite rules are applied in this way, the order of application can be important; so in practice you will want the source query to **ORDER BY** some ordering key.

   Consider a real-life astronomical example. We will expand query supernovae using table-driven rewriting rules:

   ::

      CREATE TABLE tsearch.aliases (id int, t tsquery, s tsquery);

      INSERT INTO tsearch.aliases VALUES(1, to_tsquery('supernovae'), to_tsquery('supernovae|sn'));

      SELECT ts_rewrite(to_tsquery('supernovae & crab'), 'SELECT t, s FROM tsearch.aliases');
                 ts_rewrite
      ---------------------------------
       'crab' & ( 'supernova' | 'sn' )
      (1 row)

   We can change the rewriting rules just by updating the table:

   ::

      UPDATE tsearch.aliases
      SET s = to_tsquery('supernovae|sn & !nebulae')
      WHERE t = to_tsquery('supernovae');

      SELECT ts_rewrite(to_tsquery('supernovae & crab'), 'SELECT t, s FROM tsearch.aliases');
                       ts_rewrite
      ---------------------------------------------
       'crab' & ( 'supernova' | 'sn' & !'nebula' )
      (1 row)

   Rewriting can be slow when there are many rewriting rules, since it checks every rule for a possible match. To filter out obvious non-candidate rules we can use the containment operators for the **tsquery** type. In the example below, we select only those rules which might match the original query:

   ::

      SELECT ts_rewrite('a & b'::tsquery, 'SELECT t,s FROM tsearch.aliases WHERE ''a & b''::tsquery @> t');
       ts_rewrite
      ------------
       'b' & 'a'
      (1 row)
      DROP TABLE ts_rewrite;
