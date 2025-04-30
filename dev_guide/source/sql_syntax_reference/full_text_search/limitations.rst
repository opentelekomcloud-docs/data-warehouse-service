:original_name: dws_06_0115.html

.. _dws_06_0115:

Limitations
===========

The current limitations of GaussDB(DWS)'s full text search are:

-  The length of each lexeme must be less than 2 KB.
-  The length of a **tsvector** (lexemes + positions) must be less than 1 megabyte.
-  Position values in **tsvector** must be greater than 0 and no more than 16,383.
-  No more than 256 positions per lexeme. Excessive positions, if any, will be discarded.
-  The number of nodes (lexemes + operators) in a tsquery must be less than 32,768.
