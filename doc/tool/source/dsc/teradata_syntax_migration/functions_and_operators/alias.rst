:original_name: dws_16_0052.html

.. _dws_16_0052:

.. _en-us_topic_0000001860318749:

ALIAS
=====

**ALIAS** is supported by all databases. In Teradata, an **ALIAS** can be referred in **SELECT** and **WHERE** clauses of the same statement where the alias is defined. Since **ALIAS** is not supported in **SELECT** and **WHERE** clauses in the target, it is replaced by the defined value/expression.

.. note::

   -  The comparison operators LT, LE, GT, GE, EQ, and NE must not be used as TABLE alias or COLUMN alias.

   -  The tool supports column ALIAS. If the ALIAS is the same as the column name, the ALIAS is specified only for that column and not for other columns in the table. In the following example, the column name **DATA_DT** conflicts with the alias **DATA_DT**, which is not supported by the tool.

      ::

         SELECT DATA_DT,DATA_INT AS DATA_DT FROM KK WHERE DATA_DT=DATE;

**Input: ALIAS**

::

   SELECT
             expression1 (
                  TITLE 'Expression 1'
             ) AS alias1
             ,CASE
                  WHEN alias1 + Cx >= z
                  THEN 1
                  ELSE 0
             END AS alias2
        FROM
             tab1
        WHERE
             alias1 = y
   ;

**Output:** **tdMigrateALIAS = FALSE**

::

   SELECT
             expression1 AS alias1
             ,CASE
                  WHEN alias1 + Cx >= z
                  THEN 1
                  ELSE 0
             END AS alias2
        FROM
             tab1
        WHERE
             alias1 = y
   ;

**Output:** **tdMigrateALIAS = TRUE**

::

   SELECT
             expression1 AS alias1
             ,CASE
                  WHEN expression1 + Cx >= z
                  THEN 1
                  ELSE 0
             END AS alias2
        FROM
             tab1
        WHERE
             expression1 = y
   ;
