:original_name: dws_04_0529.html

.. _dws_04_0529:

Executing Dynamic Non-query Statements
======================================

Syntax
------

:ref:`Figure 1 <en-us_topic_0000001510162925__ff22be8f7560147fb8e4d56af6224d63c>` shows the syntax diagram.

.. _en-us_topic_0000001510162925__ff22be8f7560147fb8e4d56af6224d63c:

.. figure:: /_static/images/en-us_image_0000001510284021.png
   :alt: **Figure 1** noselect::=

   **Figure 1** noselect::=

:ref:`Figure 2 <en-us_topic_0000001510162925__f0f94a40afad6436aa7bdec9de6505226>` shows the syntax diagram for **using_clause**.

.. _en-us_topic_0000001510162925__f0f94a40afad6436aa7bdec9de6505226:

.. figure:: /_static/images/en-us_image_0000001460723192.png
   :alt: **Figure 2** using_clause-2

   **Figure 2** using_clause-2

The above syntax diagram is explained as follows:

**USING IN bind_argument** is used to specify the variable that transfers values to dynamic SQL statements. It is used when a placeholder exists in **dynamic_noselect_string**. That is, a placeholder is replaced by the corresponding *bind_argument* when a dynamic SQL statement is executed. Note that *bind_argument* can only be a value, variable, or expression, and cannot be a database object such as a table name, column name, and data type. If a stored procedure needs to transfer database objects through *bind_argument* to construct dynamic SQL statements (generally, DDL statements), you are advised to use double vertical bars (||) to concatenate *dynamic_select_clause* with a database object. In addition, a dynamic PL/SQL block allows duplicate placeholders. That is, a placeholder can correspond to only one *bind_argument*.

Examples
--------

::

   -- Create a table:
   CREATE TABLE sections_t1
   (
      section       NUMBER(4) ,
      section_name  VARCHAR2(30),
      manager_id    NUMBER(6),
      place_id      NUMBER(4)
   )
   DISTRIBUTE BY hash(manager_id);

   --Declare a variable:
   DECLARE
      section       NUMBER(4) := 280;
      section_name  VARCHAR2(30) := 'Info support';
      manager_id    NUMBER(6) := 103;
      place_id      NUMBER(4) := 1400;
      new_colname   VARCHAR2(10) := 'sec_name';
   BEGIN
   -- Execute the query:
       EXECUTE IMMEDIATE 'insert into sections_t1 values(:1, :2, :3, :4)'
          USING section, section_name, manager_id,place_id;
   -- Execute the query (duplicate placeholders):
       EXECUTE IMMEDIATE 'insert into sections_t1 values(:1, :2, :3, :1)'
          USING section, section_name, manager_id;
   -- Run the ALTER statement. (You are advised to use double vertical bars (||) to concatenate the dynamic DDL statement with a database object.)
       EXECUTE IMMEDIATE 'alter table sections_t1 rename section_name to ' || new_colname;
   END;
   /

   -- Query data:
   SELECT * FROM sections_t1;

   --Delete the table.
   DROP TABLE sections_t1;
