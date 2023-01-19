:original_name: dws_06_0088.html

.. _dws_06_0088:

Searching a Table
=================

It is possible to do a full text search without an index.

-  A simple query to print each row that contains the word **science** in its **body** column is as follows:

   ::

      DROP SCHEMA IF EXISTS tsearch CASCADE;

      CREATE SCHEMA tsearch;

      CREATE TABLE tsearch.pgweb(id int, body text, title text, last_mod_date date);

      INSERT INTO tsearch.pgweb VALUES(1, 'Philology is the study of words, especially the history and development of the words in a particular language or group of languages.', 'Philology', '2010-1-1');

      INSERT INTO tsearch.pgweb VALUES(2, 'Mathematics is the science that deals with the logic of shape, quantity and arrangement.', 'Mathematics', '2010-1-1');

      INSERT INTO tsearch.pgweb VALUES(3, 'Computer science is the study of processes that interact with data and that can be represented as data in the form of programs.', 'Computer science', '2010-1-1');

      INSERT INTO tsearch.pgweb VALUES(4, 'Chemistry is the scientific discipline involved with elements and compounds composed of atoms, molecules and ions.', 'Chemistry', '2010-1-1');

      INSERT INTO tsearch.pgweb VALUES(5, 'Geography is a field of science devoted to the study of the lands, features, inhabitants, and phenomena of the Earth and planets.', 'Geography', '2010-1-1');

      INSERT INTO tsearch.pgweb VALUES(6, 'History is a subject studied in schools, colleges, and universities that deals with events that have happened in the past.', 'History', '2010-1-1');

      INSERT INTO tsearch.pgweb VALUES(7, 'Medical science is the science of dealing with the maintenance of health and the prevention and treatment of disease.', 'Medical science', '2010-1-1');

      INSERT INTO tsearch.pgweb VALUES(8, 'Physics is one of the most fundamental scientific disciplines, and its main goal is to understand how the universe behaves.', 'Physics', '2010-1-1');


      SELECT id, body, title FROM tsearch.pgweb WHERE to_tsvector('english', body) @@ to_tsquery('english', 'science');
       id |                                                          body                                                           |  title
      ----+-------------------------------------------------------------------------------------------------------------------------+---------

       2 | Mathematics is the science that deals with the logic of shape, quantity and arrangement.                                        | Mathematics
       3 | Computer science is the study of processes that interact with data and that can be represented as data in the form of programs. | Computer science
       5 | Geography is a field of science devoted to the study of the lands, features, inhabitants, and phenomena of the Earth and planets.   | Geography
       7 | Medical science is the science of dealing with the maintenance of health and the prevention and treatment of disease.           | Medical science
      (4 rows)

   This will also find related words, such as **science**, since all these are reduced to the same normalized lexeme.

   The query above specifies that the **english** configuration is to be used to parse and normalize the strings. Alternatively we could omit the configuration parameters, and use the configuration set by **default_text_search_config**.

   ::

      SHOW default_text_search_config;
       default_text_search_config
      ----------------------------
       pg_catalog.english
      (1 row)

      SELECT id, body, title FROM tsearch.pgweb WHERE to_tsvector(body) @@ to_tsquery('science');
       id |                                                          body                                                           |  title
      ----+-------------------------------------------------------------------------------------------------------------------------+---------

       2 | Mathematics is the science that deals with the logic of shape, quantity and arrangement.                                        | Mathematics
       3 | Computer science is the study of processes that interact with data and that can be represented as data in the form of programs. | Computer science
       5 | Geography is a field of science devoted to the study of the lands, features, inhabitants, and phenomena of the Earth and planets.   | Geography
       7 | Medical science is the science of dealing with the maintenance of health and the prevention and treatment of disease.           | Medical science

      (4 rows)

-  A more complex example to select the ten most recent documents that contain **treatment** and **science** in the **title** or **body** column is as follows:

   ::

      SELECT title FROM tsearch.pgweb WHERE to_tsvector(title || ' ' || body) @@ to_tsquery('treatment & science') ORDER BY last_mod_date DESC LIMIT 10;
       title
      --------

      Medical science

      (1 rows)

   For clarity we omitted the **coalesce** function calls which would be needed to find rows that contain **NULL** in one of the two columns.

   The preceding examples show queries without using indexes. Most applications will find this approach too slow. Therefore, practical use of text searching usually requires creating an index, except perhaps for occasional ad-hoc searches.
