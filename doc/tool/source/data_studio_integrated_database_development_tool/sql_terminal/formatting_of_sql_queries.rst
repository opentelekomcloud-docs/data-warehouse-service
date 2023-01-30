:original_name: DWS_DS_124.html

.. _DWS_DS_124:

Formatting of SQL Queries
=========================

Data Studio supports formatting and highlighting of SQL queries and PL/SQL statements.

.. _en-us_topic_0000001145833047__en-us_topic_0185264768_section45362064:

PL/SQL Formatting
-----------------

Follow the steps to format PL/SQL statements:

#. Select the PL/SQL statements to be formatted.

#. Click\ |image1| on the toolbar to format the query.

   Alternatively, use the key combination **Ctrl+Shift+F** or choose **Edit > Format** from the main menu.

   The PL/SQL statements are formatted.

SQL Formatting
--------------

Data Studio supports formatting of simple SQL SELECT, INSERT, UPDATE, DELETE statements which are syntactically correct. The following are some of the statements for which formatting is supported:

#. The SELECT statement must be made of the following clauses:

   -  Target list
   -  From clause (includes join)
   -  Where clause
   -  Group by clause
   -  Having clause
   -  Order by clause
   -  Common table expression

   SELECT statement without SET operations like UNION, UNION ALL, MINUS, INTERSECT and so on.

   SELECT statements without sub-queries.

#. The INSERT statement is made of the following clauses only:

   -  Insert Into Table name
   -  Values clause
   -  Values Column List
   -  RETURNING

#. The UPDATE statement is made of the following clauses only:

   -  Update Table name
   -  SET Clause
   -  From Clause(Including Join)
   -  Where Clause
   -  RETURNING

#. The DELETE statement is made of the following clauses only:

   -  Delete From Table name
   -  Using Clause(Including Join)
   -  Where Clause
   -  RETURNING

Follow the steps below to format SQL queries:

#. Select the SQL query statements to be formatted.

#. Click\ |image2| on the toolbar to format the query.

   Alternatively, use the key combination **Ctrl+Shift+F** or choose **Edit > Format** from the main menu.

   The query is formatted.

   Refer following table for query formatting rules.

   .. table:: **Table 1** Query formatting rules

      +-----------+--------------------------+--------------------------------------------+
      | Statement | Clauses                  | Formatting Rules                           |
      +===========+==========================+============================================+
      | SELECT    | SELECT list              | Line break before first column             |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent column list                         |
      +-----------+--------------------------+--------------------------------------------+
      |           | FROM                     | Line break before FROM                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after FROM                      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent FROM list                           |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack FROM list                            |
      +-----------+--------------------------+--------------------------------------------+
      |           | JOIN (FROM clause)       | Line break before JOIN                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after JOIN                      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before ON                       |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after ON                        |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent table after JOIN                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent ON condition                        |
      +-----------+--------------------------+--------------------------------------------+
      |           | WHERE                    | Line break before WHERE                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after WHERE                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent WHERE condition                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Place WHERE condition on single line       |
      +-----------+--------------------------+--------------------------------------------+
      |           | GROUP BY                 | Line break before GROUP                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before GROUP BY expression      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent column list                         |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack column list                          |
      +-----------+--------------------------+--------------------------------------------+
      |           | HAVING                   | Line break before HAVING                   |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after HAVING                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent HAVING condition                    |
      +-----------+--------------------------+--------------------------------------------+
      |           | ORDER BY                 | Line break before ORDER                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after BY                        |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent column list                         |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack column list                          |
      +-----------+--------------------------+--------------------------------------------+
      |           | CTE                      | Indent subquery braces                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Each CTE in a new line                     |
      +-----------+--------------------------+--------------------------------------------+
      | INSERT    | INSERT INFO              | Line break before opening brace            |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after opening brace             |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before closing brace            |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent column list braces                  |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent column list                         |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before VALUES                   |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack column list                          |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before VALUES                   |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before opening brace            |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after opening brace             |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before closing brace            |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent VALUES expressions list braces      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent VALUES expressions list             |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack VALUES expressions list              |
      +-----------+--------------------------+--------------------------------------------+
      |           | DEFAULT                  | Line break before DEFAULT                  |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent DEFAULT keyword                     |
      +-----------+--------------------------+--------------------------------------------+
      |           | CTE                      | Each CTE in a new line                     |
      +-----------+--------------------------+--------------------------------------------+
      |           | RETURNING                | Line break before RETURNING                |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after RETURNING                 |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent RETURNING column list               |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Place RETURNING column List on single line |
      +-----------+--------------------------+--------------------------------------------+
      | UPDATE    | UPDATE Table             | Line break before table                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent table                               |
      +-----------+--------------------------+--------------------------------------------+
      |           | SET Clause               | Line break before SET                      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent column assignments list             |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent column assignments list             |
      +-----------+--------------------------+--------------------------------------------+
      |           | FROM CLAUSE              | Line break before FROM                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after FROM                      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent FROM list                           |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack FROM list                            |
      +-----------+--------------------------+--------------------------------------------+
      |           | JOIN CLAUSE(FROM CLAUSE) | Line break before JOIN                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after JOIN                      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before ON                       |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after ON                        |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent table after JOIN                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent ON condition                        |
      +-----------+--------------------------+--------------------------------------------+
      |           | WHERE CLAUSE             | Line break before WHERE                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after WHERE                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent WHERE condition                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent WHERE condition                     |
      +-----------+--------------------------+--------------------------------------------+
      |           | CTE                      | Each CTE in a new line                     |
      +-----------+--------------------------+--------------------------------------------+
      |           | RETURNING                | Line break before RETURNING                |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after RETURNING                 |
      +-----------+--------------------------+--------------------------------------------+
      | DELETE    | USING CLAUSE             | Indent RETURNING column list               |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before FROM                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after FROM                      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent USING list                          |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack FROM list                            |
      +-----------+--------------------------+--------------------------------------------+
      |           | JOIN CLAUSE              | Line break before JOIN                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after JOIN                      |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break before ON                       |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after ON                        |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent table after JOIN                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent ON condition List                   |
      +-----------+--------------------------+--------------------------------------------+
      |           | WHERE CLAUSE             | Line break before WHERE                    |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after WHERE                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent WHERE condition                     |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Stack WHERE condition list                 |
      +-----------+--------------------------+--------------------------------------------+
      |           | CTE                      | Each CTE in a new line                     |
      +-----------+--------------------------+--------------------------------------------+
      |           | RETURNING                | Line break before RETURNING                |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Line break after RETURNING                 |
      +-----------+--------------------------+--------------------------------------------+
      |           |                          | Indent RETURNING column list               |
      +-----------+--------------------------+--------------------------------------------+

Data Studio supports automatic highlighting of the following punctuation mark's pair when cursor is placed before or after the punctuation mark or the punctuation mark is selected.

-  Brackets - ( )
-  Square brackets - [ ]
-  Braces - { }
-  Single-quoted string literals - ' '
-  Double-quoted string literals - " "

Follow the steps below to change case for SQL queries and PL/SQL statements:

Text case can be changed in the SQL Terminal using one of the following methods:

**Method 1:**

#. Select the text, and choose **Edit > Upper Case/Lower Case**.

   The text changes to the case selected.

**Method 2:**

#. Select the text, and choose |image3|  or |image4|\ from the toolbar.

   The text changes to the case selected.

**Method 3:**

#. Select the text, and press **Ctrl+Shift+U** to change to the upper case or **Ctrl+Shift+L** to change to the lower case.

   The text changes to the case selected.

SQL Highlighting
----------------

Keywords are highlighted automatically when you enter them (according to the default color scheme) as shown below:

|image5|

The following figure shows the default color scheme for the specified type of syntax:

|image6|

Refer to :ref:`Syntax Highlighting <en-us_topic_0000001145913107__en-us_topic_0185264581_section6791101652013>` to customize the SQL highlighting color scheme for the specific type of syntax.

.. |image1| image:: /_static/images/en-us_image_0000001145713327.jpg
.. |image2| image:: /_static/images/en-us_image_0000001099153386.jpg
.. |image3| image:: /_static/images/en-us_image_0000001098673576.jpg
.. |image4| image:: /_static/images/en-us_image_0000001145913369.jpg
.. |image5| image:: /_static/images/en-us_image_0000001098673574.jpg
.. |image6| image:: /_static/images/en-us_image_0000001145713325.jpg
