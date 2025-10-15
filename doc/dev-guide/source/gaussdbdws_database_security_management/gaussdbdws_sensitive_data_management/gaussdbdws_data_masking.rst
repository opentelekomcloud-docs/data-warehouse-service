:original_name: dws_04_0062.html

.. _dws_04_0062:

GaussDB(DWS) Data Masking
=========================

GaussDB(DWS) provides the column-level dynamic data masking (DDM) function. For sensitive data (such as the ID card number, mobile number, and bank card number), the DDM function is used to redact the original data to protect data security and user privacy.

-  Creating a data masking policy for a table

   GaussDB(DWS) uses the **CREATE REDACTION POLICY** syntax to create a data masking policy on a table (Do not perform masking), **MASK_FULL** (Mask data into a fixed value), and **MASK_PARTIAL** (Perform partial masking based on the character type, numeric type, or time type.) to specify the application scope of the masking policy.

-  Modifying the data masking policy of a table

   The **ALTER REDACTION POLICY** syntax is used to modify the expression for enabling a masking policy, rename a masking policy, and add, modify, or delete masked columns.

-  Deleting the masking policy of a table

   The **DROP REDACTION POLICY** syntax is used to delete the masking function information of a masking policy on all columns of a table.

-  Viewing the masking policy and masked columns

   Masking policy information is stored in the system catalog :ref:`PG_REDACTION_POLICY <dws_04_0611>`, and masked column information is stored in the system catalog :ref:`PG_REDACTION_COLUMN <dws_04_0610>`. You can view information about the masking policy and masked columns in the system views :ref:`REDACTION_POLICIES <dws_04_0858>` and :ref:`REDACTION_COLUMNS <dws_04_0857>`.

.. note::

   -  Generally, you can run the SELECT statement to view the data masking result. If a statement has the following features, sensitive data may be deliberately obtained. In this case, an error will be reported during statement execution.

      -  The GROUP BY clause references the Target Entry containing masked columns as the target column.
      -  DISTINCT works on the output masked columns.
      -  The statement contains CTE.
      -  Operations on sets are involved.
      -  The target columns of a subquery are not masked columns of the base table, but the expressions or function calls for masked columns of the base table.

   -  You can use COPY TO or GDS to export the masked data. Due to the irreversibility of the data masking, secondary masking of the data is meaningless.
   -  Do not set target columns of UPDATE, MERGE INTO, and DELETE statements to masked columns.
   -  The UPSERT statement allows you to insert update data through EXCLUDED. If data in the base table is updated by referencing masked columns, the data may be modified by mistake. As a result, an error will be reported during the execution.
   -  In the 8.2.1 cluster version, multiple masking policies can be created for the same table to implement diversified sensitive data classification. The principles for selecting and applying masking policies are as follows:

      -  Select the policy with the largest **policy_order** among multiple candidate policies that meet the requirements of the current session. A larger **policy_order** indicates a later creation.
      -  During data masking, the DML statement inherits only the policy with the largest **policy_order**.

Examples
--------

The following uses the employee table **emp**, table owner **alice**, and roles **matu** and **july** as an example to illustrate the data masking process. The **emp** table contains private data such as the employee name, mobile number, email address, bank card number, and salary.

#. After connecting to the database as the administrator, create roles **alice**, **matu**, and **july**.

   ::

      CREATE ROLE alice PASSWORD 'password';
      CREATE ROLE matu PASSWORD 'password';
      CREATE ROLE july PASSWORD 'password';

#. Grant schema permissions on the current database to **alice**, **matu**, and **july**.

   ::

      GRANT ALL PRIVILEGES on schema public to alice,matu,july;

#. Switch to role **alice**, create the **emp** table, and insert three pieces of employee information.

   ::

      SET ROLE alice PASSWORD 'password';

      CREATE TABLE emp(id int, name varchar(20), phone_no varchar(11), card_no number, card_string varchar(19), email text, salary numeric(100, 4), birthday date);

      INSERT INTO emp VALUES(1, 'anny', '13420002340', 1234123412341234, '1234-1234-1234-1234', 'smithWu@163.com', 10000.00, '1999-10-02');
      INSERT INTO emp VALUES(2, 'bob', '18299023211', 3456345634563456, '3456-3456-3456-3456', '66allen_mm@qq.com', 9999.99, '1989-12-12');
      INSERT INTO emp VALUES(3, 'cici', '15512231233', NULL, NULL, 'jonesishere@sina.com', NULL, '1992-11-06');

#. **alice** grants the read permission on the **emp** table to **matu** and **july**.

   ::

      GRANT SELECT ON emp TO matu, july;

#. Create the masking policy **mask_emp**: Only user **alice** can view all employee information. User **matu** and **july** cannot view employee bank card numbers and salary data. The **card_no** column is of the numeric type and all of its data is masked into 0 by the **MASK_FULL** function. The **card_string** column is of the character type and part of its data is masked by the **MASK_PARTIAL** function based on the specified input and output formats. The **salary** column is of the numeric type and the **MASK_PARTIAL** function is used to mask all digits before the penultimate digit using the number 9.

   ::

      CREATE REDACTION POLICY mask_emp ON emp WHEN (current_user IN ('matu', 'july'))
       ADD COLUMN card_no WITH mask_full(card_no),
       ADD COLUMN card_string WITH mask_partial(card_string, 'VVVVFVVVVFVVVVFVVVV','VVVV-VVVV-VVVV-VVVV','#',1,12),
       ADD COLUMN salary WITH mask_partial(salary, '9', 1, length(salary) - 2);

#. Switch to **matu** and **july** and view the employee table **emp**.

   ::

      SET ROLE matu PASSWORD 'password';
      SELECT * FROM emp;
       id | name |  phone_no   | card_no |     card_string     |        email         |   salary   |      birthday
      ----+------+-------------+---------+---------------------+----------------------+------------+---------------------
        1 | anny | 13420002340 |       0 | ####-####-####-1234 | smithWu@163.com      | 99999.9990 | 1999-10-02 00:00:00
        2 | bob  | 18299023211 |       0 | ####-####-####-3456 | 66allen_mm@qq.com    |  9999.9990 | 1989-12-12 00:00:00
        3 | cici | 15512231233 |         |                     | jonesishere@sina.com |            | 1992-11-06 00:00:00
      (3 rows)

      SET ROLE july PASSWORD 'password';
      SELECT * FROM emp;
       id | name |  phone_no   | card_no |     card_string     |        email         |   salary   |      birthday
      ----+------+-------------+---------+---------------------+----------------------+------------+---------------------
        1 | anny | 13420002340 |       0 | ####-####-####-1234 | smithWu@163.com      | 99999.9990 | 1999-10-02 00:00:00
        2 | bob  | 18299023211 |       0 | ####-####-####-3456 | 66allen_mm@qq.com    |  9999.9990 | 1989-12-12 00:00:00
        3 | cici | 15512231233 |         |                     | jonesishere@sina.com |            | 1992-11-06 00:00:00
      (3 rows)

#. If you want **matu** to have the permission to view all employee information, but do not want **july** to have. In this case, you only need to modify the effective scope of the policy.

   ::

      SET ROLE alice PASSWORD 'password';
      ALTER REDACTION POLICY mask_emp ON emp WHEN(current_user = 'july');

#. Switch to users **matu** and **july** and view the **emp** table again, respectively.

   ::

      SET ROLE matu PASSWORD 'password';
      SELECT * FROM emp;
       id | name |  phone_no   |     card_no      |     card_string     |        email         |   salary   |      birthday
      ----+------+-------------+------------------+---------------------+----------------------+------------+---------------------
        1 | anny | 13420002340 | 1234123412341234 | 1234-1234-1234-1234 | smithWu@163.com      | 10000.0000 | 1999-10-02 00:00:00
        2 | bob  | 18299023211 | 3456345634563456 | 3456-3456-3456-3456 | 66allen_mm@qq.com    |  9999.9900 | 1989-12-12 00:00:00
        3 | cici | 15512231233 |                  |                     | jonesishere@sina.com |            | 1992-11-06 00:00:00
      (3 rows)

      SET ROLE july PASSWORD 'password';
      SELECT * FROM emp;
       id | name |  phone_no   | card_no |     card_string     |        email         |   salary   |      birthday
      ----+------+-------------+---------+---------------------+----------------------+------------+---------------------
        1 | anny | 13420002340 |       0 | ####-####-####-1234 | smithWu@163.com      | 99999.9990 | 1999-10-02 00:00:00
        2 | bob  | 18299023211 |       0 | ####-####-####-3456 | 66allen_mm@qq.com    |  9999.9990 | 1989-12-12 00:00:00
        3 | cici | 15512231233 |         |                     | jonesishere@sina.com |            | 1992-11-06 00:00:00
      (3 rows)

#. The information in the **phone_no**, **email**, and **birthday** columns is private data. Update masking policy **mask_emp** and add three masked columns.

   ::

      SET ROLE alice PASSWORD 'password';
      ALTER REDACTION POLICY mask_emp ON emp ADD COLUMN phone_no WITH mask_partial(phone_no, '*', 4);
      ALTER REDACTION POLICY mask_emp ON emp ADD COLUMN email WITH mask_partial(email, '*', 1, position('@' in email));
      ALTER REDACTION POLICY mask_emp ON emp ADD COLUMN birthday WITH mask_full(birthday);

#. Switch to **july** and view data in the **emp** table.

   ::

      SET ROLE july PASSWORD 'password';
      SELECT * FROM emp;
       id | name |  phone_no   | card_no |     card_string     |        email         |   salary   |      birthday
      ----+------+-------------+---------+---------------------+----------------------+------------+---------------------
        1 | anny | 134******** |       0 | ####-####-####-1234 | ********163.com      | 99999.9990 | 1970-01-01 00:00:00
        2 | bob  | 182******** |       0 | ####-####-####-3456 | ***********qq.com    |  9999.9990 | 1970-01-01 00:00:00
        3 | cici | 155******** |         |                     | ************sina.com |            | 1970-01-01 00:00:00
      (3 rows)

#. Query **redaction_policies** and **redaction_columns** to view details about the current redaction policy **mask_emp**.

   ::

      SELECT * FROM redaction_policies;
       object_schema | object_owner | object_name | policy_name |            expression             | enable | policy_description | inherited
      ---------------+--------------+-------------+-------------+-----------------------------------+--------+--------------------+-----------
       public        | alice        | emp         | mask_emp    | ("current_user"() = 'july'::name) | t      |                    | f
      (1 row)

      SELECT object_name, column_name, function_info FROM redaction_columns;
       object_name | column_name |                                             function_info
      -------------+-------------+-------------------------------------------------------------------------------------------------------
       emp         | card_no     | mask_full(card_no)
       emp         | card_string | mask_partial(card_string, 'VVVVFVVVVFVVVVFVVVV'::text, 'VVVV-VVVV-VVVV-VVVV'::text, '#'::text, 1, 12)
       emp         | email       | mask_partial(email, '*'::text, 1, "position"(email, '@'::text))
       emp         | salary      | mask_partial(salary, '9'::text, 1, (length((salary)::text) - 2))
       emp         | birthday    | mask_full(birthday)
       emp         | phone_no    | mask_partial(phone_no, '*'::text, 4)
      (6 rows)

#. Add the **salary_info** column. To replace the salary information in text format with \*.*, you can create a user-defined masking function. In this step, you can use the PL/pgSQL to define the masking function **mask_regexp_salary**. To create a masking column, you simply need to customize the function name and parameter list. For details, see :ref:`GaussDB(DWS) User-Defined Functions <dws_04_0507>`.

   ::

      SET ROLE alice PASSWORD 'password';

      ALTER TABLE emp ADD COLUMN salary_info TEXT;
      UPDATE emp SET salary_info = salary::text;

      CREATE FUNCTION mask_regexp_salary(salary_info text) RETURNS text AS
      $$
       SELECT regexp_replace($1, '[0-9]+','*','g');
      $$
       LANGUAGE SQL
      STRICT SHIPPABLE;

      ALTER REDACTION POLICY mask_emp ON emp ADD COLUMN salary_info WITH mask_regexp_salary(salary_info);

      SET ROLE july PASSWORD 'password';
      SELECT id, name, salary_info FROM emp;
       id | name | salary_info
      ----+------+-------------
        1 | anny | *.*
        2 | bob  | *.*
        3 | cici |
      (3 rows)

#. If there is no need to set a redaction policy for the **emp** table, delete redaction policy **mask_emp**.

   ::

      SET ROLE alice PASSWORD 'password';
      DROP REDACTION POLICY mask_emp ON emp;
