:original_name: dws_06_0071.html

.. _dws_06_0071:

Conditional Expressions
=======================

Data that meets the requirements specified by conditional expressions are filtered during SQL statement execution.

Conditional expressions include the following types:

-  CASE

   **CASE** expressions are similar to the **CASE** statements in other coding languages.

   :ref:`Figure 1 <en-us_topic_0000001764516522__f01d218ade2304aa39904ab7270de148d>` shows the syntax of a **CASE** expression.

   .. _en-us_topic_0000001764516522__f01d218ade2304aa39904ab7270de148d:

   .. figure:: /_static/images/en-us_image_0000001811634945.jpg
      :alt: **Figure 1** case::=

      **Figure 1** case::=

   A **CASE** clause can be used in a valid expression. **condition** is an expression that returns a value of Boolean type.

   -  If the result is **true**, the result of the **CASE** expression is the required result.
   -  If the result is false, the following **WHEN** or **ELSE** clauses are processed in the same way.
   -  If every **WHEN condition** is false, the result of the expression is the result of the **ELSE** clause. If the **ELSE** clause is omitted and has no match condition, the result is NULL.

   Examples:

   ::

      CREATE TABLE tpcds.case_when_t1(CW_COL1 INT)  DISTRIBUTE BY HASH (CW_COL1);

      INSERT INTO tpcds.case_when_t1 VALUES (1), (2), (3);

      SELECT * FROM tpcds.case_when_t1;
       cw_col1
      ---------
             3
             1
             2
      (3 rows)

      SELECT CW_COL1, CASE WHEN CW_COL1=1 THEN 'one' WHEN CW_COL1=2 THEN 'two' ELSE 'other' END FROM tpcds.case_when_t1;
       cw_col1 | case
      ---------+-------
             3 | other
             1 | one
             2 | two
      (3 rows)

      DROP TABLE tpcds.case_when_t1;

-  DECODE

   :ref:`Figure 2 <en-us_topic_0000001764516522__fd14d2a98b6614a1990d3a7745dd91b21>` shows the syntax of a **DECODE** expression.

   .. _en-us_topic_0000001764516522__fd14d2a98b6614a1990d3a7745dd91b21:

   .. figure:: /_static/images/en-us_image_0000001764675566.png
      :alt: **Figure 2** decode::=

      **Figure 2** decode::=

   Compare each following **compare(n)** with **base_expr**, **value(n)** is returned if a **compare(n)** matches the **base_expr** expression. If **base_expr** does not match each **compare(n)**, the default value is returned.

   :ref:`Conditional Expression Functions <dws_06_0050>` describes the examples.

   ::

      SELECT DECODE('A','A',1,'B',2,0);
       case
      ------
          1
      (1 row)

-  COALESCE

   :ref:`Figure 3 <en-us_topic_0000001764516522__f49993c20aa1746a1bcd8dc6fc33f1129>` shows the syntax of a **COALESCE** expression.

   .. _en-us_topic_0000001764516522__f49993c20aa1746a1bcd8dc6fc33f1129:

   .. figure:: /_static/images/en-us_image_0000001764675562.png
      :alt: **Figure 3** coalesce::=

      **Figure 3** coalesce::=

   **COALESCE** returns its first non-NULL value. If all the arguments are NULL, return **NULL**. This value is replaced by the default value when data is displayed. Like a **CASE** expression, **COALESCE** only evaluates the parameters that are needed to determine the result. That is, parameters to the right of the first non-null parameter are not evaluated.

   The following is an example:

   ::

      CREATE TABLE tpcds.c_tabl(description varchar(10), short_description varchar(10), last_value varchar(10))
      DISTRIBUTE BY HASH (last_value);

      INSERT INTO tpcds.c_tabl VALUES('abc', 'efg', '123');
      INSERT INTO tpcds.c_tabl VALUES(NULL, 'efg', '123');

      INSERT INTO tpcds.c_tabl VALUES(NULL, NULL, '123');

      SELECT description, short_description, last_value, COALESCE(description, short_description, last_value) FROM tpcds.c_tabl ORDER BY 1, 2, 3, 4;
       description | short_description | last_value | coalesce
      -------------+-------------------+------------+----------
       abc         | efg               | 123        | abc
                   | efg               | 123        | efg
                   |                   | 123        | 123
      (3 rows)

      DROP TABLE tpcds.c_tabl;

   If **description** is not **NULL**, the value of **description** is returned. Otherwise, parameter **short_description** is calculated. If **short_description** is not **NULL**, the value of **short_description** is returned. Otherwise, parameter **last_value** is calculated. If **last_value** is not **NULL**, the value of **last_value** is returned. Otherwise, **none** is returned.

   ::

      SELECT COALESCE(NULL,'Hello World');
         coalesce
      ---------------
       Hello World
      (1 row)

-  NULLIF

   :ref:`Figure 4 <en-us_topic_0000001764516522__f08785065f90f4fcf836fcc8d88b56686>` shows the syntax of a **NULLIF** expression.

   .. _en-us_topic_0000001764516522__f08785065f90f4fcf836fcc8d88b56686:

   .. figure:: /_static/images/en-us_image_0000001811515869.png
      :alt: **Figure 4** nullif::=

      **Figure 4** nullif::=

   Only if **value1** is equal to **value2** can **NULLIF** return the **NULL** value. Otherwise, **value1** is returned.

   The following is an example:

   ::

      CREATE TABLE tpcds.null_if_t1 (
          NI_VALUE1 VARCHAR(10),
          NI_VALUE2 VARCHAR(10)
      )  DISTRIBUTE BY HASH (NI_VALUE1);

      INSERT INTO tpcds.null_if_t1 VALUES('abc', 'abc');
      INSERT INTO tpcds.null_if_t1 VALUES('abc', 'efg');

      SELECT NI_VALUE1, NI_VALUE2, NULLIF(NI_VALUE1, NI_VALUE2) FROM tpcds.null_if_t1 ORDER BY 1, 2, 3;

       ni_value1 | ni_value2 | nullif
      -----------+-----------+--------
       abc       | abc       |
       abc       | efg       | abc
      (2 rows)
      DROP TABLE tpcds.null_if_t1;

   If **value1** is equal to **value2**, **NULL** is returned. Otherwise, **value1** is returned.

   ::

      SELECT NULLIF('Hello','Hello World');
       nullif
      --------
       Hello
      (1 row)

-  GREATEST (maximum value) and LEAST (minimum value)

   :ref:`Figure 5 <en-us_topic_0000001764516522__f7837163f04e147fdbf313fc02759293e>` shows the syntax of a **GREATEST** expression.

   .. _en-us_topic_0000001764516522__f7837163f04e147fdbf313fc02759293e:

   .. figure:: /_static/images/en-us_image_0000001811515877.png
      :alt: **Figure 5** greatest::=

      **Figure 5** greatest::=

   You can select the maximum value from any numerical expression list.

   ::

      SELECT greatest(9000,155555,2.01);
       greatest
      ----------
         155555
      (1 row)

   :ref:`Figure 6 <en-us_topic_0000001764516522__f7c82b3fb53a74e5b928072c3e971408a>` shows the syntax of a **LEAST** expression.

   .. _en-us_topic_0000001764516522__f7c82b3fb53a74e5b928072c3e971408a:

   .. figure:: /_static/images/en-us_image_0000001764675570.png
      :alt: **Figure 6** least::=

      **Figure 6** least::=

   You can select the minimum value from any numerical expression list.

   Each of the preceding numeric expressions can be converted into a common data type, which will be the data type of the result.

   The NULL values in the list will be ignored. The result is **NULL** only if the results of all expressions are **NULL**.

   ::

      SELECT least(9000,2);
       least
      -------
           2
      (1 row)

   :ref:`Conditional Expression Functions <dws_06_0050>` describes the examples.

-  NVL

   :ref:`Figure 7 <en-us_topic_0000001764516522__f94614e31e7e046d7842d729dbd442f86>` shows the syntax of an **NVL** expression.

   .. _en-us_topic_0000001764516522__f94614e31e7e046d7842d729dbd442f86:

   .. figure:: /_static/images/en-us_image_0000001764516602.jpg
      :alt: **Figure 7** nvl::=

      **Figure 7** nvl::=

   If the value of **value1** is **NULL**, **value2** is returned. Otherwise, **value1** is returned.

   For example:

   ::

      SELECT nvl(null,1);
      nvl
      -----
       1
      (1 row)

   ::

      SELECT nvl ('Hello World' ,1);
            nvl
      ---------------
       Hello World
      (1 row)

-  IF

   :ref:`Figure 8 <en-us_topic_0000001764516522__fig591124561511>` shows the syntax of an **IF** expression.

   .. _en-us_topic_0000001764516522__fig591124561511:

   .. figure:: /_static/images/en-us_image_0000001811634941.png
      :alt: **Figure 8** if::=

      **Figure 8** if::=

   If the value of **bool_expr** is **true**, **expr1** is returned. Otherwise, **expr2** is returned.

   :ref:`Conditional Expression Functions <dws_06_0050>` describes the examples.

-  IFNULL

   :ref:`Figure 9 <en-us_topic_0000001764516522__fig294554533118>` shows the syntax of a **NULLIF** expression.

   .. _en-us_topic_0000001764516522__fig294554533118:

   .. figure:: /_static/images/en-us_image_0000001764516598.png
      :alt: **Figure 9** ifnull::=

      **Figure 9** ifnull::=

   It returns **expr1** or **expr2**. If **expr1** is not **NULL**, **expr1** is returned. Otherwise, **expr2** is returned.

   :ref:`Conditional Expression Functions <dws_06_0050>` describes the examples.
