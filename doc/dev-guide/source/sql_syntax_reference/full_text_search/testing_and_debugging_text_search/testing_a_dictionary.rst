:original_name: dws_06_0114.html

.. _dws_06_0114:

Testing a Dictionary
====================

The **ts_lexize** function facilitates dictionary testing.

**ts_lexize(dict regdictionary, token text) returns text[]** **ts_lexize** returns an array of lexemes if the input **token** is known to the dictionary, or an empty array if the token is known to the dictionary but it is a stop word, or **NULL** if it is an unknown word.

For example:

::

   SELECT ts_lexize('english_stem', 'stars');
    ts_lexize
   -----------
    {star}

   SELECT ts_lexize('english_stem', 'a');
    ts_lexize
   -----------
    {}

.. important::

   The **ts_lexize** function expects a single **token**, not text.
