:original_name: dws_06_0107.html

.. _dws_06_0107:

Thesaurus Dictionary
====================

A thesaurus dictionary (sometimes abbreviated as TZ) is a collection of words that include relationships between words and phrases, such as broader terms (BT), narrower terms (NT), preferred terms, non-preferred terms, and related terms. A thesaurus dictionary replaces all non-preferred terms by one preferred term and, optionally, preserves the original terms for indexing as well. A thesaurus dictionary is an extension of the synonym dictionary with added phrase support.

Precautions
-----------

-  A thesaurus dictionary has the capability to recognize phrases. Therefore, it must remember its state and interact with the parser to check whether it should handle the next token or stop accumulation. The thesaurus dictionary must be configured carefully. For example, if the thesaurus dictionary is assigned to handle only the **asciiword** token, then a thesaurus dictionary definition like **one 7** will not work because token type **uint** is not assigned to the thesaurus dictionary.
-  Thesauruses are used during indexing. Any change in the thesaurus dictionary's parameters requires reindexing. For most other dictionary types, small changes such as adding or removing stop words does not force reindexing.

Procedure
---------

#. Create a TZ named **thesaurus_astro**.

   **thesaurus_astro** is a simple astronomical TZ that defines two astronomical word combinations (word+synonym).

   ::

      supernovae stars : sn
      crab nebulae : crab

   Run the following statement to create the TZ:

   .. important::

      // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

   ::

      CREATE TEXT SEARCH DICTIONARY thesaurus_astro (
          TEMPLATE = thesaurus,
          DictFile = thesaurus_astro,
          Dictionary = pg_catalog.english_stem,
          FILEPATH = 'obs://bucket_name/path accesskey=ak secretkey=sk region=rg'
      );

   The full name of the thesaurus dictionary file is **thesaurus_astro.ths**, and the dictionary is stored in the **obs://bucket_name/path accesskey=ak secretkey=sk region=rg** directory. **pg_catalog.english_stem** is the subdictionary (a **Snowball** English stemmer) used for input normalization. The subdictionary has its own configuration (for example, stop words), which is not shown here. For details about the syntax and parameters for creating a TZ, see :ref:`CREATE TEXT SEARCH DICTIONARY <dws_06_0183>`.

#. Bind the TZ to the desired token types in the text search configuration.

   ::

      ALTER TEXT SEARCH CONFIGURATION english
          ALTER MAPPING FOR asciiword, asciihword, hword_asciipart
          WITH thesaurus_astro, english_stem;

#. Use the TZ.

   -  Test the TZ.

      The **ts_lexize** function is not very useful for testing the TZ because the function processes its input as a single token. Instead, you can use the **plainto_tsquery**, **to_tsvector**, or **to_tsquery** function which will break their input strings into multiple tokens.

      ::

         SELECT plainto_tsquery('english','supernova star');
          plainto_tsquery
         -----------------
          'sn'
         (1 row)

         SELECT to_tsvector('english','supernova star');
          to_tsvector
         -------------
          'sn':1
         (1 row)

         SELECT to_tsquery('english','''supernova star''');
          to_tsquery
         ------------
          'sn'
         (1 row)

      **supernova star** matches **supernovae stars** in **thesaurus_astro** because the **english_stem** stemmer is specified in the **thesaurus_astro** definition. The stemmer removed **e** and **s**.

   -  To index the original phrase, include it in the right-hand part of the definition.

      ::

         supernovae stars : sn supernovae stars

         ALTER TEXT SEARCH DICTIONARY thesaurus_astro (
             DictFile = thesaurus_astro,
             FILEPATH = 'file:///home/dicts/');

         SELECT plainto_tsquery('english','supernova star');
                plainto_tsquery
         -----------------------------
          'sn' & 'supernova' & 'star'
         (1 row)
