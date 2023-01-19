:original_name: dws_06_0085.html

.. _dws_06_0085:

Basic Text Matching
===================

Full text search in GaussDB(DWS) is based on the match operator **@@**, which returns **true** if a **tsvector** (document) matches a **tsquery** (query). It does not matter which data type is written first:

::

   SELECT 'a fat cat sat on a mat and ate a fat rat'::tsvector @@ 'cat & rat'::tsquery AS RESULT;
    result
   ----------
    t
   (1 row)

::

   SELECT 'fat & cow'::tsquery @@ 'a fat cat sat on a mat and ate a fat rat'::tsvector AS RESULT;
    result
   ----------
    f
   (1 row)

As the above example suggests, a **tsquery** is not raw text, any more than a **tsvector** is. A tsquery contains search terms, which must be already-normalized lexemes, and may combine multiple terms using **AND**, **OR**, and **NOT** operators. For details, see :ref:`Text Search Types <dws_06_0018>`. There are functions **to_tsquery** and **plainto_tsquery** that are helpful in converting user-written text into a proper tsquery, for example by normalizing words appearing in the text. Similarly, **to_tsvector** is used to parse and normalize a document string. So in practice a text search match would look more like this:

::

   SELECT to_tsvector('fat cats ate fat rats') @@ to_tsquery('fat & rat') AS RESULT;
   result
   ----------
    t
   (1 row)

Observe that this match would not succeed if written as follows:

::

   SELECT 'fat cats ate fat rats'::tsvector @@ to_tsquery('fat & rat')AS RESULT;
   result
   ----------
    f
   (1 row)

In the preceding match, no normalization of the word **rats** will occur. Therefore, **rats** does not match **rat**.

The **@@** operator also supports text input, allowing explicit conversion of a text string to **tsvector** or **tsquery** to be skipped in simple cases. The variants available are:

::

   tsvector @@ tsquery
   tsquery  @@ tsvector
   text @@ tsquery
   text @@ text

We already saw the first two of these. The form **text @@ tsquery** is equivalent to **to_tsvector(text) @@ tsquery**. The form **text @@ text** is equivalent to **to_tsvector(text) @@ plainto_tsquery(text)**.
