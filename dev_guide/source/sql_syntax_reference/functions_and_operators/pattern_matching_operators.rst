:original_name: dws_06_0033.html

.. _dws_06_0033:

Pattern Matching Operators
==========================

There are three separate approaches to pattern matching provided by the database: the traditional SQL LIKE operator, the more recent SIMILAR TO operator, and POSIX-style regular expressions. Besides these basic operators, functions can be used to extract or replace matching substrings and to split a string at matching locations.

-  LIKE

   Description: checks whether the string matches the mode string following LIKE. The LIKE expression returns true if the string matches the supplied pattern. (As expected, the NOT LIKE expression returns false if LIKE returns true, and vice versa.

   Matching rule:

   #. This operator can succeed only when its pattern matches the entire string. If you want to match a sequence in any position within the string, the pattern must begin and end with a percent sign.
   #. The underscore (_) represents (matching) any single character. Percentage (%) indicates the wildcard character of any string.
   #. To match a literal underscore or percent sign without matching other characters, the respective character in pattern must be preceded by the escape character. The default escape character is the backslash but a different one can be selected by using the ESCAPE clause.
   #. To match the escape character itself, write two escape characters. For example: To write a **pattern** constant containing a backslash (\\), you need to enter two backslashes in SQL statements.

      .. note::

         When **standard_conforming_strings** is set to **off**, any backslashes you write in literal string constants will need to be doubled. Therefore, writing a pattern matching a single backslash is actually going to write four backslashes in the statement. You can avoid this by selecting a different escape character by using ESCAPE, so that the backslash is no longer a special character of LIKE. But the backslash is still the special character of the character text analyzer, so you still need two backslashes.) You can also select no escape character by writing **ESCAPE ''**. This effectively disables the escape mechanism, which makes it impossible to turn off the special meaning of underscore and percent signs in the pattern.

   #. The keyword ILIKE can be used instead of LIKE to make the match case-insensitive.
   #. Operator ~~ is equivalent to LIKE, and operator ~~\* corresponds to ILIKE.

   For example:

   ::

      SELECT 'abc' LIKE 'abc' AS RESULT;
       result
      -----------
       t
      (1 row)

   ::

      SELECT 'abc' LIKE 'a%' AS RESULT;
       result
      -----------
       t
      (1 row)

   ::

      SELECT 'abc' LIKE '_b_' AS RESULT;
       result
      -----------
       t
      (1 row)

   ::

      SELECT 'abc' LIKE 'c' AS RESULT;
       result
      -----------
       f
      (1 row)

-  SIMILAR TO

   Description: The SIMILAR TO operator returns true or false depending on whether the pattern matches the given string. It is similar to LIKE, except that it interprets the pattern using the SQL standard's definition of a regular expression.

   Matching rule:

   #. Like LIKE, this operator succeeds only when its pattern matches the entire string. If you want to match a sequence in any position within the string, the pattern must begin and end with a percent sign.
   #. The underscore (_) represents (matching) any single character. Percentage (%) indicates the wildcard character of any string.
   #. SIMILAR TO supports these pattern-matching metacharacters borrowed from POSIX regular expressions:

      +---------------+---------------------------------------------------------------------------------------------+
      | Metacharacter | Description                                                                                 |
      +===============+=============================================================================================+
      | \|            | Specifies alternation (either of two alternatives).                                         |
      +---------------+---------------------------------------------------------------------------------------------+
      | \*            | Specifies repetition of the previous item zero or more times.                               |
      +---------------+---------------------------------------------------------------------------------------------+
      | +             | Specifies repetition of the previous item one or more times.                                |
      +---------------+---------------------------------------------------------------------------------------------+
      | ?             | Specifies repetition of the previous item zero or one time.                                 |
      +---------------+---------------------------------------------------------------------------------------------+
      | {m}           | Specifies repetition of the previous item exactly *m* times.                                |
      +---------------+---------------------------------------------------------------------------------------------+
      | {m,}          | Specifies repetition of the previous item *m* or more times.                                |
      +---------------+---------------------------------------------------------------------------------------------+
      | {m,n}         | Specifies repetition of the previous item at least *m* times and does not exceed *n* times. |
      +---------------+---------------------------------------------------------------------------------------------+
      | ()            | Specifies that parentheses () can be used to group items into a single logical item.        |
      +---------------+---------------------------------------------------------------------------------------------+
      | [...]         | Specifies a character class, just as in POSIX regular expressions.                          |
      +---------------+---------------------------------------------------------------------------------------------+

   #. A preamble escape character disables the special meaning of any of these metacharacters. The rules for using escape characters are the same as those for LIKE.

   Regular expressions:

   The substring function with three parameters, :ref:`substring(string from pattern for escape) <en-us_topic_0000001145830881__l0e44fce441ff4e4fb16e5dc766f2978d>`, provides extraction of a substring that matches an SQL regular expression pattern.

   Example:

   ::

      SELECT 'abc' SIMILAR TO 'abc' AS RESULT;
       result
      -----------
       t
      (1 row)

   ::

      SELECT 'abc' SIMILAR TO 'a' AS RESULT;
       result
      -----------
       f
      (1 row)

   ::

      SELECT 'abc' SIMILAR TO '%(b|d)%' AS RESULT;
       result
      -----------
       t
      (1 row)

   ::

      SELECT 'abc' SIMILAR TO '(b|c)%'  AS RESULT;
       result
      -----------
       f
      (1 row)

-  POSIX regular expressions

   Description: A regular expression is a character sequence that is an abbreviated definition of a set of strings (a regular set). If a string is a member of a regular expression described by a regular expression, the string matches the regular expression. POSIX regular expressions provide a more powerful means for pattern matching than the LIKE and SIMILAR TO operators. :ref:`Table 1 Regular expression match operators <en-us_topic_0000001098830830__table6512684711360>` lists all available operators for pattern matching using POSIX regular expressions.

   .. _en-us_topic_0000001098830830__table6512684711360:

   .. table:: **Table 1** Regular expression match operators

      +----------+---------------------------------------------------------------+---------------------------+
      | Operator | Description                                                   | Example                   |
      +==========+===============================================================+===========================+
      | ~        | Matches regular expression, which is case-sensitive.          | 'thomas' ~ '.*thomas.*'   |
      +----------+---------------------------------------------------------------+---------------------------+
      | ~\*      | Matches regular expression, which is case-insensitive.        | 'thomas' ~\* '.*Thomas.*' |
      +----------+---------------------------------------------------------------+---------------------------+
      | ! ~      | Does not match regular expression, which is case-sensitive.   | 'thomas' !~ '.*Thomas.*'  |
      +----------+---------------------------------------------------------------+---------------------------+
      | ! ~\*    | Does not match regular expression, which is case-insensitive. | 'thomas' !~\* '.*vadim.*' |
      +----------+---------------------------------------------------------------+---------------------------+

   Matching rule:

   #. Unlike LIKE patterns, a regular expression is allowed to match anywhere within a string, unless the regular expression is explicitly anchored to the beginning or end of the string.
   #. Besides the metacharacters mentioned above, POSIX regular expressions also support the following pattern matching metacharacters:

      ============= ===========================================
      Metacharacter Description
      ============= ===========================================
      ^             Specifies the match starting with a string.
      $             Specifies the match at the end of a string.
      .             Matches any single character.
      ============= ===========================================

   Regular expressions:

   POSIX regular expressions support the following functions:

   -  The :ref:`substring(string from pattern) <en-us_topic_0000001145830881__ld8dcea80ff7a4f82a31eab8e9396ea4c>` function provides a method for extracting a substring that matches the POSIX regular expression pattern.
   -  The :ref:`regexp_replace <en-us_topic_0000001145830881__le93bdaa34c0441c28759ceb4bdc06897>` function provides the function of replacing the substring matching the POSIX regular expression pattern with the new text.
   -  The :ref:`regexp_matches <en-us_topic_0000001145830881__l20ce459cad154e07b802f6795ec11003>` function returns a text array consisting of all captured substrings that match a POSIX regular expression pattern.
   -  The :ref:`regexp_split_to_table <en-us_topic_0000001145830881__l012d5649840e4577bba0dc409dc23ac4>` function splits a string using a POSIX regular expression pattern as a delimiter.
   -  The :ref:`regexp_split_to_array <en-us_topic_0000001145830881__l74d8354e578c490797f3a6e0668107a9>` function behaves the same as regexp_split_to_table, except that regexp_split_to_array returns its result as an array of text.

      .. note::

         The regular expression split functions ignore zero-length matches, which occur at the beginning or end of a string or after the previous match. This is contrary to the strict definition of regular expression matching. The latter is implemented by regexp_matches, but the former is usually the most commonly used behavior in practice.

   For example:

   ::

       SELECT 'abc' ~ 'Abc' AS RESULT;
      result
      --------
       f
      (1 row)

   ::

      SELECT 'abc' ~* 'Abc' AS RESULT;
       result
      --------
       t
      (1 row)

   ::

      SELECT 'abc' !~ 'Abc' AS RESULT;
       result
      --------
       t
      (1 row)

   ::

      SELECT 'abc'!~* 'Abc' AS RESULT;
       result
      --------
       f
      (1 row)

   ::

      SELECT 'abc' ~ '^a' AS RESULT;
       result
      --------
       t
      (1 row)

   ::

      SELECT 'abc' ~ '(b|d)'AS RESULT;
       result
      --------
       t
      (1 row)

   ::

      SELECT 'abc' ~ '^(b|c)'AS RESULT;
       result
      --------
       f
      (1 row)

   Although most regular expression searches can be executed quickly, the time and memory for regular expression processing can still be manually controlled. It is not recommended that you accept the regular expression search mode from the non-security mode source. If you must do this, you are advised to add the statement timeout limit. The search with the SIMILAR TO mode has the same security risks as the SIMILAR TO provides many capabilities that are the same as those of the POSIX- style regular expression. The LIKE search is much simpler than the other two options. Therefore, it is more secure to accept the non-secure mode source search.
