:original_name: dws_16_0095.html

.. _dws_16_0095:

.. _en-us_topic_0000001813598944:

MERGE
=====

**MERGE** is an ANSI-standard SQL syntax operator used to select rows from one or more sources for updating or inserting into a table or view. The conditions to update or insert to the target table or view can be specified.

**Input: MERGE**

::

   MERGE INTO tab1 A
   using ( SELECT c1, c2, ... FROM tab2 WHERE ...) AS B
   ON A.c1 = B.c1
    WHEN MATCHED THEN
      UPDATE SET c2 = c2
               , c3 = c3
     WHEN NOT MATCHED THEN
   INSERT VALUES (B.c1, B.c2, B.c3);

**Output**

::

   WITH B AS (
        SELECT
                  c1
                  ,c2
                  ,...
             FROM
                  tab2
             WHERE
                  ...
   )
   ,UPD_REC AS (
        UPDATE
                  tab1 A
             SET
                  c2 = c2
                  ,c3 = c3
             FROM
                  B
             WHERE
                  A.c1 = B.c1 returning A. *
   )
   INSERT
        INTO
             tab1 SELECT
                       B.c1
                       ,B.c2
                       ,B.c3
                    FROM
                       B
                   WHERE
                       NOT EXISTS (
                            SELECT
                                    1
                              FROM
                                    UPD_REC A
                             WHERE
                                    A.c1 = B.c1
                                  )
   ;
