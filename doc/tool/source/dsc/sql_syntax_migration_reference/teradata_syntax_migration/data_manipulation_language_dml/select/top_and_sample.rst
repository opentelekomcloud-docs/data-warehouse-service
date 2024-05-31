:original_name: dws_16_0092.html

.. _dws_16_0092:

.. _en-us_topic_0000001772536464:

TOP and SAMPLE
==============

The TOP and SAMPLE clauses of Teradata are migrated to LIMIT in GaussDB(DWS).

TOP
---

The DSC also supports migration of **TOP** statements with dynamic parameters.

.. note::

   -  For **TOP** clauses containing **WITH TIES**, the ORDER BY clause is also required. Otherwise, the tool will not migrate the statement and copy it as it is.
   -  When using TOP with dynamic parameters:

      -  The input dynamic parameters should be in the following form:

         ::

             TOP :<parameter_name>

         The following characters are valid for dynamic parameters: a-z, A-Z, 0-9 and "_".

**Input: SELECT .. TOP**

::

   SELECT TOP 1 c1, COUNT (*) cnt
     FROM tab1
    GROUP BY c1
    ORDER BY cnt;

**Output**

::

   SELECT c1, COUNT( * ) cnt
     FROM tab1
    GROUP BY c1
    ORDER BY cnt
    LIMIT 1;

**Input:** **SELECT .. TOP** **PERCENT**

::

   SELECT TOP 10 PERCENT c1, c2
     FROM employee
    WHERE ...
    ORDER BY c2 DESC;

**Output**

::

   WITH top_percent AS (
         SELECT c1, c2
           FROM employee
          WHERE ...
          ORDER BY c2 DESC
                       )
   SELECT *
     FROM top_percent
    LIMIT (SELECT CEIL(COUNT( * ) * 10 / 100)
             FROM top_percent);

**Input:** **SELECT .. TOP with** **dynamic parameters**

::

   SELECT
              TOP :Limit WITH TIES c1
             ,SUM (c2) sc2
        FROM
             tab1
        WHERE
             c3 > 10
        GROUP BY
             c1
        ORDER BY
             c1
   ;

**Output**

::

   WITH top_ties AS (
        SELECT
                   c1
                  ,SUM (c2) sc2
                  ,rank (
                  ) OVER( ORDER BY c1 ) AS TOP_RNK
             FROM
                  tab1
             WHERE
                  c3 > 10
             GROUP BY
                  c1
   ) SELECT
             c1
             ,sc2
        FROM
             top_ties
        WHERE
             TOP_RNK <= :Limit
        ORDER BY
             TOP_RNK
   ;

**Input:** **SELECT .. TOP with** **dynamic parameters and with TIES**

::

    SELECT
              TOP :Limit WITH TIES Customer_ID
      FROM
             Customer_t
      ORDER BY
             Customer_ID
   ;

**Output**

::

   WITH top_ties AS (
        SELECT
                  Customer_ID
                  ,rank (
                  ) OVER( order by Customer_id) AS TOP_RNK
             FROM
                  Customer_t
   ) SELECT
             Customer_ID
        FROM
             top_ties
        WHERE
             TOP_RNK <= :Limit
        ORDER BY
             TOP_RNK
   ;

**Input:** **SELECT .. TOP PERCENT with** **dynamic parameters**

::

   SELECT
             TOP :Input_Limit PERCENT WITH TIES c1
             ,SUM (c2) sc2
        FROM
             tab1
        GROUP BY
             c1
        ORDER BY
             c1
   ;

**Output**

::

   WITH top_percent_ties AS (
        SELECT
                  c1
                  ,SUM (c2) sc2
                  ,rank (
                  ) OVER( ORDER BY c1 ) AS TOP_RNK
             FROM
                  tab1
             GROUP BY
                  c1
   ) SELECT
             c1
             ,sc2
        FROM
             top_percent_ties
        WHERE
             TOP_RNK <= (
                  SELECT
                            CEIL(COUNT( * ) * :Input_Limit / 100)
                       FROM
                            top_percent_ties
             )
        ORDER BY
             TOP_RNK
   ;

SAMPLE
------

.. note::

   The tool only supports single positive integers in the SAMPLE clause.

**Input:** **SELECT .. SAMPLE**

::

   SELECT c1, c2, c3
     FROM tab1
    WHERE c1 > 1000
   SAMPLE 1;

**Output**

::

   SELECT c1, c2, c3
     FROM tab1
    WHERE c1 > 1000
    LIMIT 1;
