:original_name: dws_04_0530.html

.. _dws_04_0530:

Dynamically Calling Stored Procedures
=====================================

This section describes how to dynamically call store procedures. You must use anonymous statement blocks to package stored procedures or statement blocks and append **IN** and **OUT** behind the **EXECUTE IMMEDIATE...USING** statement to input and output parameters.

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001811490929__f0349b2280c1b49a59991a3d738298c77>` shows the syntax diagram.

.. _en-us_topic_0000001811490929__f0349b2280c1b49a59991a3d738298c77:

.. figure:: /_static/images/en-us_image_0000001764492252.png
   :alt: **Figure 1** call_procedure::=

   **Figure 1** call_procedure::=

:ref:`Figure 2 <en-us_topic_0000001811490929__f07ede7782f5c41afa548256a382c493a>` shows the syntax diagram for **using_clause**.

.. _en-us_topic_0000001811490929__f07ede7782f5c41afa548256a382c493a:

.. figure:: /_static/images/en-us_image_0000001811491517.png
   :alt: **Figure 2** using_clause-3

   **Figure 2** using_clause-3

The above syntax diagram is explained as follows:

-  **CALL procedure_name**: calls the stored procedure.
-  **[:placeholder1,:placeholder2, ...]**: specifies the placeholder list of the stored procedure parameters. The number of placeholders is the same as the number of parameters. A placeholder name starts with a colon (:) or dollar sign ($). The colon (:) can be followed by digits, characters, or character strings (excluding digits, characters, or character strings with quotation marks). The dollar sign ($) can be followed only by digits. A placeholder can correspond to only one *bind_argument* in the **USING** clause.
-  **USING [IN|OUT|IN OUT]bind_argument**: specifies where the variable passed to the stored procedure parameter value is stored. The modifiers in front of **bind_argument** and of the corresponding parameter are the same.

Examples
--------

::

   --Create the stored procedure proc_add.
   CREATE OR REPLACE PROCEDURE proc_add
   (
       param1    in   INTEGER,
       param2    out  INTEGER,
       param3    in   INTEGER
   )
   AS
   BEGIN
      param2:= param1 + param3;
   END;
   /

   DECLARE
       input1 INTEGER:=1;
       input2 INTEGER:=2;
       statement  VARCHAR2(200);
       param2     INTEGER;
   BEGIN
      --Declare the call statement.
       statement := 'call proc_add(:col_1, :col_2, :col_3)';(or statement := 'call proc_add($1, $2, $3)';)
      --Execute the statement.
       EXECUTE IMMEDIATE statement
           USING IN input1, OUT param2, IN input2;
       dbms_output.put_line('result is: '||to_char(param2));
   END;
   /

   --Delete the stored procedure.
   DROP PROCEDURE proc_add;
