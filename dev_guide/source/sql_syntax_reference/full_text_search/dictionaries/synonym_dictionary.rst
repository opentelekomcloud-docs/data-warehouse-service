:original_name: dws_06_0106.html

.. _dws_06_0106:

Synonym Dictionary
==================

A synonym dictionary is used to define, identify, and convert synonyms of tokens. Phrases are not supported (use the thesaurus dictionary in :ref:`Thesaurus Dictionary <dws_06_0107>`).

Examples
--------

-  A synonym dictionary can be used to overcome linguistic problems, for example, to prevent an English stemmer dictionary from reducing the word "Paris" to "pari". It is enough to have a **Paris paris** line in the synonym dictionary and put it before the **english_stem** dictionary.

   .. important::

      // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

   ::

      SELECT * FROM ts_debug('english', 'Paris');
         alias   |   description   | token |  dictionaries  |  dictionary  | lexemes
      -----------+-----------------+-------+----------------+--------------+---------
       asciiword | Word, all ASCII | Paris | {english_stem} | english_stem | {pari}
      (1 row)

      CREATE TEXT SEARCH DICTIONARY my_synonym (
          TEMPLATE = synonym,
          SYNONYMS = my_synonyms,
          FILEPATH =   'obs://bucket01/obs.xxx.xxx.com accesskey=xxxxx secretkey=xxxxx region=xx-xx-xx'
      );

      ALTER TEXT SEARCH CONFIGURATION english
          ALTER MAPPING FOR asciiword
          WITH my_synonym, english_stem;

      SELECT * FROM ts_debug('english', 'Paris');
         alias   |   description   | token |       dictionaries        | dictionary | lexemes
      -----------+-----------------+-------+---------------------------+------------+---------
       asciiword | Word, all ASCII | Paris | {my_synonym,english_stem} | my_synonym | {paris}
      (1 row)

      SELECT * FROM ts_debug('english', 'paris');
         alias   |   description   | token |       dictionaries        | dictionary | lexemes
      -----------+-----------------+-------+---------------------------+------------+---------
       asciiword | Word, all ASCII | Paris | {my_synonym,english_stem} | my_synonym | {paris}
      (1 row)

      ALTER TEXT SEARCH DICTIONARY my_synonym ( CASESENSITIVE=true);

      SELECT * FROM ts_debug('english', 'Paris');
         alias   |   description   | token |       dictionaries        | dictionary | lexemes
      -----------+-----------------+-------+---------------------------+------------+---------
       asciiword | Word, all ASCII | Paris | {my_synonym,english_stem} | my_synonym | {paris}
      (1 row)

      SELECT * FROM ts_debug('english', 'paris');
         alias   |   description   | token |       dictionaries        | dictionary | lexemes
      -----------+-----------------+-------+---------------------------+------------+---------
       asciiword | Word, all ASCII | Paris | {my_synonym,english_stem} | my_synonym | {pari}
      (1 row)

   The full name of the synonym dictionary file is **my_synonyms.syn**, and the dictionary is stored in the **obs://bucket01/obs.xxx.xxx.com accesskey=xxxxx secretkey=xxxxx region=\ xx-xx-xx** directory. For details about the syntax and parameters for creating a synonym dictionary, see :ref:`CREATE TEXT SEARCH DICTIONARY <dws_06_0183>`.

-  An asterisk (*) can be placed at the end of a synonym in the configuration file. This indicates that the synonym is a prefix. The asterisk is ignored when the entry is used in to_tsvector(), but when it is used in to_tsquery(), the result will be a query item with the prefix match marker (see :ref:`Handling TSQuery <dws_06_0098>`).

   Assume that the content in the dictionary file **synonym_sample.syn** is as follows:

   ::

      postgres        pgsql
      postgresql      pgsql
      postgre pgsql
      gogle   googl
      indices index*

   Create and use a dictionary.

   ::

      CREATE TEXT SEARCH DICTIONARY syn (
          TEMPLATE = synonym,
          SYNONYMS = synonym_sample
      );

      SELECT ts_lexize('syn','indices');
       ts_lexize
      -----------
       {index}
      (1 row)

      CREATE TEXT SEARCH CONFIGURATION tst (copy=simple);

      ALTER TEXT SEARCH CONFIGURATION tst ALTER MAPPING FOR asciiword WITH syn;

      SELECT to_tsvector('tst','indices');
       to_tsvector
      -------------
       'index':1
      (1 row)

      SELECT to_tsquery('tst','indices');
       to_tsquery
      ------------
       'index':*
      (1 row)

      SELECT 'indexes are very useful'::tsvector;
                  tsvector
      ---------------------------------
       'are' 'indexes' 'useful' 'very'
      (1 row)

      SELECT 'indexes are very useful'::tsvector @@ to_tsquery('tst','indices');
       ?column?
      ----------
       t
      (1 row)
