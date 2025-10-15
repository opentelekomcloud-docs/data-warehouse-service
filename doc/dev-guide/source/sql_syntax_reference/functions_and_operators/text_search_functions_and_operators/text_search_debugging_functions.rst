:original_name: dws_06_0321.html

.. _dws_06_0321:

Text Search Debugging Functions
===============================

ts_debug([ config regconfig, ] document text, OUT alias text, OUT description text, OUT token text, OUT dictionaries regdictionary[], OUT dictionary regdictionary, OUT lexemes text[])
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Description: Tests a configuration.

Return type: SETOF record

Example:

::

   SELECT ts_debug('english', 'The Brightest supernovaes');
                                        ts_debug
   -----------------------------------------------------------------------------------
    (asciiword,"Word, all ASCII",The,{english_stem},english_stem,{})
    (blank,"Space symbols"," ",{},,)
    (asciiword,"Word, all ASCII",Brightest,{english_stem},english_stem,{brightest})
    (blank,"Space symbols"," ",{},,)
    (asciiword,"Word, all ASCII",supernovaes,{english_stem},english_stem,{supernova})
   (5 rows)

ts_lexize(dict regdictionary, token text)
-----------------------------------------

Description: Tests a data dictionary.

Return type: text[]

Example:

::

   SELECT ts_lexize('english_stem', 'stars');
    ts_lexize
   -----------
    {star}
   (1 row)

ts_parse(parser_name text, document text, OUT tokid integer, OUT token text)
----------------------------------------------------------------------------

Description: Tests a parser.

Return type: SETOF record

Example:

::

   SELECT ts_parse('default', 'foo - bar');
    ts_parse
   -----------
    (1,foo)
    (12," ")
    (12,"- ")
    (1,bar)
   (4 rows)

ts_parse(parser_oid oid, document text, OUT tokid integer, OUT token text)
--------------------------------------------------------------------------

Description: Tests a parser.

Return type: SETOF record

Example:

::

   SELECT ts_parse(3722, 'foo - bar');
    ts_parse
   -----------
    (1,foo)
    (12," ")
    (12,"- ")
    (1,bar)
   (4 rows)

ts_token_type(parser_name text, OUT tokid integer, OUT alias text, OUT description text)
----------------------------------------------------------------------------------------

Description: Gets token types defined by parser.

Return type: SETOF record

Example:

::

   SELECT ts_token_type('default');
                           ts_token_type
   --------------------------------------------------------------
    (1,asciiword,"Word, all ASCII")
    (2,word,"Word, all letters")
    (3,numword,"Word, letters and digits")
    (4,email,"Email address")
    (5,url,URL)
    (6,host,Host)
    (7,sfloat,"Scientific notation")
    (8,version,"Version number")
    (9,hword_numpart,"Hyphenated word part, letters and digits")
    (10,hword_part,"Hyphenated word part, all letters")
    (11,hword_asciipart,"Hyphenated word part, all ASCII")
    (12,blank,"Space symbols")
    (13,tag,"XML tag")
    (14,protocol,"Protocol head")
    (15,numhword,"Hyphenated word, letters and digits")
    (16,asciihword,"Hyphenated word, all ASCII")
    (17,hword,"Hyphenated word, all letters")
    (18,url_path,"URL path")
    (19,file,"File or path name")
    (20,float,"Decimal notation")
    (21,int,"Signed integer")
    (22,uint,"Unsigned integer")
    (23,entity,"XML entity")
   (23 rows)

ts_token_type(parser_oid oid, OUT tokid integer, OUT alias text, OUT description text)
--------------------------------------------------------------------------------------

Description: Gets token types defined by parser.

Return type: SETOF record

Example:

::

   SELECT ts_token_type(3722);
                           ts_token_type
   --------------------------------------------------------------
    (1,asciiword,"Word, all ASCII")
    (2,word,"Word, all letters")
    (3,numword,"Word, letters and digits")
    (4,email,"Email address")
    (5,url,URL)
    (6,host,Host)
    (7,sfloat,"Scientific notation")
    (8,version,"Version number")
    (9,hword_numpart,"Hyphenated word part, letters and digits")
    (10,hword_part,"Hyphenated word part, all letters")
    (11,hword_asciipart,"Hyphenated word part, all ASCII")
    (12,blank,"Space symbols")
    (13,tag,"XML tag")
    (14,protocol,"Protocol head")
    (15,numhword,"Hyphenated word, letters and digits")
    (16,asciihword,"Hyphenated word, all ASCII")
    (17,hword,"Hyphenated word, all letters")
    (18,url_path,"URL path")
    (19,file,"File or path name")
    (20,float,"Decimal notation")
    (21,int,"Signed integer")
    (22,uint,"Unsigned integer")
    (23,entity,"XML entity")
   (23 rows)

ts_stat(sqlquery text, [ weights text, ] OUT word text, OUT ndoc integer, OUT nentry integer)
---------------------------------------------------------------------------------------------

Description: Gets statistics of a **tsvector** column.

Return type: SETOF record

Example:

::

   SELECT ts_stat('select ''hello world''::tsvector');
      ts_stat
   -------------
    (world,1,1)
    (hello,1,1)
   (2 rows)
