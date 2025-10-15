:original_name: dws_06_0320.html

.. _dws_06_0320:

Text Search Functions
=====================

get_current_ts_config()
-----------------------

Description: Gets default text search configuration.

Return type: regconfig

Example:

::

   SELECT get_current_ts_config();
    get_current_ts_config
   -----------------------
    english
   (1 row)

length(tsvector)
----------------

Description: Number of lexemes in a **tsvector**-typed word.

Return type: integer

Example:

::

   SELECT length('fat:2,4 cat:3 rat:5A'::tsvector);
    length
   --------
         3
   (1 row)

numnode(tsquery)
----------------

Description: Number of lexemes plus **tsquery** operators

Return type: integer

Example:

::

   SELECT numnode('(fat & rat) | cat'::tsquery);
    numnode
   ---------
          5
   (1 row)

plainto_tsquery([ config regconfig , ] query text)
--------------------------------------------------

Description: Generates **tsquery** lexemes without punctuation.

Return type: tsquery

Example:

::

   SELECT plainto_tsquery('english', 'The Fat Rats');
    plainto_tsquery
   -----------------
    'fat' & 'rat'
   (1 row)

querytree(query tsquery)
------------------------

Description: Gets indexable part of a **tsquery**.

Return type: text

Example:

::

   SELECT querytree('foo & ! bar'::tsquery);
    querytree
   -----------
    'foo'
   (1 row)

setweight(tsvector, "char")
---------------------------

Description: Assigns weight to each element of **tsvector**.

Return type: tsvector

Example:

::

   SELECT setweight('fat:2,4 cat:3 rat:5B'::tsvector, 'A');
              setweight
   -------------------------------
    'cat':3A 'fat':2A,4A 'rat':5A
   (1 row)

strip(tsvector)
---------------

Description: Removes positions and weights from **tsvector**.

Return type: tsvector

Example:

::

   SELECT strip('fat:2,4 cat:3 rat:5A'::tsvector);
          strip
   -------------------
    'cat' 'fat' 'rat'
   (1 row)

to_tsquery([ config regconfig , ] query text)
---------------------------------------------

Description: Normalizes words and converts them to **tsquery**.

Return type: tsquery

Example:

::

   SELECT to_tsquery('english', 'The & Fat & Rats');
     to_tsquery
   ---------------
    'fat' & 'rat'
   (1 row)

to_tsvector([ config regconfig , ] document text)
-------------------------------------------------

Description: Reduces document text to **tsvector**.

Return type: tsvector

Example:

::

   SELECT to_tsvector('english', 'The Fat Rats');
      to_tsvector
   -----------------
    'fat':2 'rat':3
   (1 row)

ts_headline([ config regconfig, ] document text, query tsquery [, options text ])
---------------------------------------------------------------------------------

Description: Highlights a query match.

Return type: text

Example:

::

   SELECT ts_headline('x y z', 'z'::tsquery);
    ts_headline
   --------------
    x y <b>z</b>
   (1 row)

ts_rank([ weights float4[], ] vector tsvector, query tsquery [, normalization integer ])
----------------------------------------------------------------------------------------

Description: Ranks document for query.

Return type: float4

Example:

::

   SELECT ts_rank('hello world'::tsvector, 'world'::tsquery);
    ts_rank
   ----------
    .0607927
   (1 row)

ts_rank_cd([ weights float4[], ] vector tsvector, query tsquery [, normalization integer ])
-------------------------------------------------------------------------------------------

Description: Ranks document for query using cover density.

Return type: float4

Example:

::

   SELECT ts_rank_cd('hello world'::tsvector, 'world'::tsquery);
    ts_rank_cd
   ------------
             0
   (1 row)

ts_rewrite(query tsquery, target tsquery, substitute tsquery)
-------------------------------------------------------------

Description: Replaces **tsquery**-typed word.

Return type: tsquery

Example:

::

   SELECT ts_rewrite('a & b'::tsquery, 'a'::tsquery, 'foo|bar'::tsquery);
          ts_rewrite
   -------------------------
    'b' & ( 'foo' | 'bar' )
   (1 row)

ts_rewrite(query tsquery, select text)
--------------------------------------

Description: Replaces **tsquery** data in the target with the result of a **SELECT** command.

Return type: tsquery

Example:

::

   SELECT ts_rewrite('world'::tsquery, 'select ''world''::tsquery, ''hello''::tsquery');
    ts_rewrite
   ------------
    'hello'
   (1 row)
