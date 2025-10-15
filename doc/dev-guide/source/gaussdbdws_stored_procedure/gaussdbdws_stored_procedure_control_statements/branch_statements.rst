:original_name: dws_04_0538.html

.. _dws_04_0538:

Branch Statements
=================

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001811609917__ff24d5dd562a74f45a3fc293326312d45>` shows the syntax diagram.

.. _en-us_topic_0000001811609917__ff24d5dd562a74f45a3fc293326312d45:

.. figure:: /_static/images/en-us_image_0000001811491505.png
   :alt: **Figure 1** case_when::=

   **Figure 1** case_when::=

:ref:`Figure 2 <en-us_topic_0000001811609917__f5787784e89b34223ba6a14f07d74dcba>` shows the syntax diagram for **when_clause**.

.. _en-us_topic_0000001811609917__f5787784e89b34223ba6a14f07d74dcba:

.. figure:: /_static/images/en-us_image_0000001811610581.png
   :alt: **Figure 2** when_clause::=

   **Figure 2** when_clause::=

Parameter description:

-  **case_expression**: specifies the variable or expression.
-  **when_expression**: specifies the constant or conditional expression.
-  **statement**: specifies the statement to execute.

Examples
--------

::

   CREATE OR REPLACE PROCEDURE proc_case_branch(pi_result in integer, pi_return out integer)
   AS
       BEGIN
           CASE pi_result
               WHEN 1 THEN
                   pi_return := 111;
               WHEN 2 THEN
                   pi_return := 222;
               WHEN 3 THEN
                   pi_return := 333;
               WHEN 6 THEN
                   pi_return := 444;
               WHEN 7 THEN
                   pi_return := 555;
               WHEN 8 THEN
                   pi_return := 666;
               WHEN 9 THEN
                   pi_return := 777;
               WHEN 10 THEN
                   pi_return := 888;
               ELSE
                   pi_return := 999;
           END CASE;
           raise info 'pi_return : %',pi_return ;
   END;
   /

   CALL proc_case_branch(3,0);

   --Delete the stored procedure.
   DROP PROCEDURE proc_case_branch;
