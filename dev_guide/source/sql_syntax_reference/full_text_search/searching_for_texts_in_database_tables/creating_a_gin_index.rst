:original_name: dws_06_0089.html

.. _dws_06_0089:

Creating a Gin Index
====================

You can create a **GIN** index to speed up text searches:

::

   CREATE INDEX pgweb_idx_1 ON tsearch.pgweb USING gin(to_tsvector('english', body));

The to_tsvector() function accepts one or two augments.

If the one-augment version of the index is used, the system will use the configuration specified by **default_text_search_config** by default.

To create a Gin index, the two-augment version must be used, otherwise the index content may be inconsistent. Only text search functions that specify a configuration name can be used in expression indexes. Index content is not affected by **default_text_search_config**, because different entries could contain **tsvector**\ s that were created with different text search configurations, and there would be no way to guess which was which. It would be impossible to dump and restore such an index correctly.

Because the two-argument version of **to_tsvector** was used in the index above, only a query reference that uses the two-argument version of **to_tsvector** with the same configuration name will use that index. That is, **WHERE to_tsvector('english', body) @@ 'a & b'** can use the index, but **WHERE to_tsvector(body) @@ 'a & b'** cannot. This ensures that an index will be used only with the same configuration used to create the index entries.

More complex expression indexes can be set up when the configuration name of the index is specified by another column. For example:

::

   CREATE INDEX pgweb_idx_2 ON tsearch.pgweb USING gin(to_tsvector('zhparser', body));

.. note::

   In this example, zhparser supports only the UTF-8 or GBK database encoding format. If the SQL_ASCII encoding is used, an error will be reported.

where **body** is a column in the **pgweb** table. This allows mixed configurations in the same index while recording which configuration was used for each index entry. This can be useful when the document collection contains documents in different languages. Again, queries that are meant to use the index must be phrased to match, for example, **WHERE to_tsvector(config_name, body) @@ 'a & b'** must match **to_tsvector** in the index.

Indexes can even concatenate columns:

::

   CREATE INDEX pgweb_idx_3 ON tsearch.pgweb USING gin(to_tsvector('english', title || ' ' || body));

Another approach is to create a separate **tsvector** column to hold the output of **to_tsvector**. This example is a concatenation of **title** and **body**, using **coalesce** to ensure that one column will still be indexed when the other is **NULL**:

::

   ALTER TABLE tsearch.pgweb ADD COLUMN textsearchable_index_col tsvector;
   UPDATE tsearch.pgweb SET textsearchable_index_col = to_tsvector('english', coalesce(title,'') || ' ' || coalesce(body,''));

Then, create a GIN index to speed up the search:

::

   CREATE INDEX textsearch_idx_4 ON tsearch.pgweb USING gin(textsearchable_index_col);

Now you are ready to perform a fast full text search:

::

   SELECT title
   FROM tsearch.pgweb
   WHERE textsearchable_index_col @@ to_tsquery('science & Computer')
   ORDER BY last_mod_date DESC
   LIMIT 10;

    title
   --------
    Computer science
   (1 rows)

One advantage of the separate-column approach over an expression index is that it is unnecessary to explicitly specify the text search configuration in queries in order to use the index. As shown in the preceding example, the query can depend on **default_text_search_config**. Another advantage is that searches will be faster, since it will not be necessary to redo the **to_tsvector** calls to verify index matches. The expression-index approach is simpler to set up, however, and it requires less disk space since the **tsvector** representation is not stored explicitly.
