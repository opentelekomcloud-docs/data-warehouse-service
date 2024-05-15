:original_name: dws_06_0093.html

.. _dws_06_0093:

Parsing Queries
===============

GaussDB(DWS) provides functions **to_tsquery** and **plainto_tsquery** for converting a query to the **tsquery** data type. **to_tsquery** offers access to more features than **plainto_tsquery**, but is less forgiving about its input.

to_tsquery
----------

**to_tsquery** converts a query to the **tsquery** data type.

.. code-block::

   to_tsquery([ config regconfig, ] querytext text) returns tsquery

**to_tsquery** creates a **tsquery** value from **querytext**, which must consist of single tokens separated by the Boolean operators **&** (AND), **\|** (OR), and **!** (NOT). These operators can be grouped using parentheses. The input to to_tsquery must follow the general rules for tsquery input, as described in :ref:`Text Search Types <dws_06_0018>`. The difference is that while basic **tsquery** input takes the tokens at face value, **to_tsquery** normalizes each token to a lexeme using the specified or default configuration, and discards any tokens that are stop words according to the configuration. For example:

::

   SELECT to_tsquery('english', 'The & Fat & Rats');
      to_tsquery
   ---------------
    'fat' & 'rat'
   (1 row)

As in basic **tsquery** input, **weight(s)** can be attached to each lexeme to restrict it to match only **tsvector** lexemes of those **weight(s)**. For example:

::

   SELECT to_tsquery('english', 'Fat | Rats:AB');
       to_tsquery
   ------------------
    'fat' | 'rat':AB
   (1 row)

In addition, the asterisk (``*``) can be attached to a lexeme to specify prefix matching.

::

   SELECT to_tsquery('supern:*A & star:A*B');
           to_tsquery
   --------------------------
    'supern':*A & 'star':*AB
   (1 row)

Such a lexeme will match any word having the specified string and weight in a **tsquery**.

plainto_tsquery
---------------

**plainto_tsquery** transforms unformatted text **querytext** to **tsquery**. The text is parsed and normalized much as for **to_tsvector**, then the **&** (AND) Boolean operator is inserted between surviving words.

.. code-block::

   plainto_tsquery([ config regconfig, ] querytext text) returns tsquery

For example:

::

   SELECT plainto_tsquery('english', 'The Fat Rats');
    plainto_tsquery
   -----------------
    'fat' & 'rat'
   (1 row)

Note that **plainto_tsquery** cannot recognize Boolean operators, weight labels, or prefix-match labels in its input:

::

   SELECT plainto_tsquery('english', 'The Fat & Rats:C');
      plainto_tsquery
   ---------------------
    'fat' & 'rat' & 'c'
   (1 row)

Here, all the input punctuation was discarded as being space symbols.
