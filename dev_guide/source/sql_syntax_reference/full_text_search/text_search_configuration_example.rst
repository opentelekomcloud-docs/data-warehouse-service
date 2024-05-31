:original_name: dws_06_0110.html

.. _dws_06_0110:

Text Search Configuration Example
=================================

Text search configuration specifies the following components required for converting a document into a **tsvector**:

-  A parser, decomposes a text into tokens.
-  Dictionary list, converts each token into a lexeme.

Each time when the **to_tsvector** or **to_tsquery** function is invoked, a text search configuration is required to specify a processing procedure. The GUC parameter **default_text_search_config** specifies the default text search configuration, which will be used if the text search function does not explicitly specify a text search configuration.

GaussDB(DWS) provides some predefined text search configurations. You can also create user-defined text search configurations. In addition, to facilitate the management of text search objects, multiple gsql meta-commands are provided to display related information. For details, see "Meta-Command Reference" in the *Tool Guide*.

Procedure
---------

#. Create a text search configuration **ts_conf** by copying the predefined text search configuration **english**.

   ::

      CREATE TEXT SEARCH CONFIGURATION ts_conf ( COPY = pg_catalog.english );
      CREATE TEXT SEARCH CONFIGURATION

#. Create a **Synonym** dictionary.

   Assume that the definition file **pg_dict.syn** of the **Synonym** dictionary contains the following contents:

   ::

      postgres    pg
      pgsql       pg
      postgresql  pg

   Run the following statement to create the **Synonym** dictionary:

   .. important::

      // Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

   ::

      CREATE TEXT SEARCH DICTIONARY pg_dict (
           TEMPLATE = synonym,
           SYNONYMS = pg_dict,
           FILEPATH =  'obs://bucket01/obs.xxx.xxx.com accesskey=xxxxx secretkey=xxxxx region=xx-xx-xx'
       );

#. Create an **Ispell** dictionary **english_ispell** (the dictionary definition file is from the open source dictionary).

   ::

      CREATE TEXT SEARCH DICTIONARY english_ispell (
          TEMPLATE = ispell,
          DictFile = english,
          AffFile = english,
          StopWords = english,
          FILEPATH =   'obs://bucket01/obs.xxx.xxx.com accesskey=xxxxx secretkey=xxxxx region=xx-xx-xx'
      );

#. Modify the text search configuration **ts_conf** and change the dictionary list for tokens of certain types. For details about token types, see :ref:`Text Search Parser <dws_06_0101>`.

   ::

      ALTER TEXT SEARCH CONFIGURATION ts_conf
          ALTER MAPPING FOR asciiword, asciihword, hword_asciipart,
                            word, hword, hword_part
          WITH pg_dict, english_ispell, english_stem;

#. In the text search configuration, set non-index or set the search for tokens of certain types.

   ::

      ALTER TEXT SEARCH CONFIGURATION ts_conf
          DROP MAPPING FOR email, url, url_path, sfloat, float;

#. Use the text retrieval commissioning function ts_debug() to test the text search configuration **ts_conf**.

   ::

      SELECT * FROM ts_debug('ts_conf', '
      PostgreSQL, the highly scalable, SQL compliant, open source object-relational
      database management system, is now undergoing beta testing of the next
      version of our software.
      ');

#. You can set the default text search configuration of the current session to **ts_conf**. This setting is valid only for the current session.

   ::

      \dF+ ts_conf
            Text search configuration "public.ts_conf"
      Parser: "pg_catalog.default"
            Token      |            Dictionaries
      -----------------+-------------------------------------
       asciihword      | pg_dict,english_ispell,english_stem
       asciiword       | pg_dict,english_ispell,english_stem
       file            | simple
       host            | simple
       hword           | pg_dict,english_ispell,english_stem
       hword_asciipart | pg_dict,english_ispell,english_stem
       hword_numpart   | simple
       hword_part      | pg_dict,english_ispell,english_stem
       int             | simple
       numhword        | simple
       numword         | simple
       uint            | simple
       version         | simple
       word            | pg_dict,english_ispell,english_stem

      SET default_text_search_config = 'public.ts_conf';
      SET
      SHOW default_text_search_config;
       default_text_search_config
      ----------------------------
       public.ts_conf
      (1 row)
