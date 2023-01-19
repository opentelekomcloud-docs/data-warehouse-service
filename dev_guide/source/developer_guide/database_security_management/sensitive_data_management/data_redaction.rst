:original_name: dws_04_0062.html

.. _dws_04_0062:

Data Redaction
==============

GaussDB(DWS) provides the column-level dynamic data masking (DDM) function. For sensitive data, such as the ID card number, mobile number, and bank card number, the DDM function is used to redact the original data to protect data security and user privacy.

-  You can create a redaction policy for a specified table object and set the effective scope of the policy. For details, see section "CREATE REDACTION POLICY" in *SQL Syntax Reference*.
-  You can modify a redaction policy using the provided syntax, including modifying expressions for the policy to take effect, renaming the policy, and adding, modifying, and deleting redacted columns. For details, see section "ALTER REDACTION POLICY" in SQL Syntax Reference.
-  You can delete a redaction policy by deleting data redaction function information of the policy from all columns of a table. For details, see "DROP REDACTION POLICY" in SQL Syntax Reference.
-  You can use the built-in masking functions **MASK_NONE**, **MASK_FULL**, and **MASK_PARTIAL**, or create your own masking functions by using PL/pgSQL. For details, see "Data Masking Functions" in *SQL Syntax Reference*.
-  Redaction policy information is stored in the system catalog :ref:`PG_REDACTION_POLICY <dws_04_0611>`, and redacted column information is stored in the system catalog :ref:`PG_REDACTION_COLUMN <dws_04_0610>`.
-  You can view information about the redaction policy and redacted columns in the system views :ref:`REDACTION_POLICIES <dws_04_0858>` and :ref:`REDACTION_COLUMNS <dws_04_0857>`.

.. note::

   -  Generally, you can execute a **SELECT** statement to view the data redaction result. If the statement has the following features, sensitive data may be deliberately obtained. In this case, an error will be reported during statement execution.

      -  The **GROUP BY** clause references a **Target Entry** that contains redacted columns as the target column.
      -  The **DISTINCT** clause is executed on the output redacted columns.
      -  The statement contains **CTE**.
      -  Set operations are involved.
      -  The target columns of a subquery are not redacted columns of the base table, but are the expressions or function calls for the redacted columns of the base table.

   -  You can use **COPY TO** or GDS to export the redacted data. As the redacted data is irreversible, any secondary operation on the redacted data is meaningless.
   -  The target columns of **UPDATE**, **MERGE INTO**, and **DELETE** statements cannot contain redacted columns.
   -  The **UPSERT** statement allows you to update data using **EXCLUDED**. If data in the base table is updated by referencing redacted columns, the data may be modified by mistake. As a result, an error will be reported during the execution.

Examples
--------

The following uses the employee table **emp**, administrator **alice**, and common users **matu** and **july** as examples to describe the data redaction process. The user **alice** is the owner of the **emp** table. The **emp** table contains private data such as the employee name, mobile number, email address, bank card number, and salary.

#. Create users **alice**, **matu**, and **july**:

   .. code-block::

      CREATE ROLE alice PASSWORD 'password';
      CREATE ROLE matu PASSWORD 'password';
      CREATE ROLE july PASSWORD 'password';

#. Create the **emp** table as user **alice**, and insert three employee records into the table.

   .. code-block::

      CREATE TABLE emp(id int, name varchar(20), phone_no varchar(11), card_no number, card_string varchar(19), email text, salary numeric(100, 4), birthday date);

      INSERT INTO emp VALUES(1, 'anny', '13420002340', 1234123412341234, '1234-1234-1234-1234', 'smithWu@163.com', 10000.00, '1999-10-02');
      INSERT INTO emp VALUES(2, 'bob', '18299023211', 3456345634563456, '3456-3456-3456-3456', '66allen_mm@qq.com', 9999.99, '1989-12-12');
      INSERT INTO emp VALUES(3, 'cici', '15512231233', NULL, NULL, 'jonesishere@sina.com', NULL, '1992-11-06');

#. User **alice** grants the **emp** table read permission to users **matu** and **july**.

   .. code-block::

      GRANT SELECT ON emp TO matu, july;

#. Only user **alice** can view all employee information. Users **matu** and **july** cannot view bank card numbers and salary data of the employees. Create a redaction policy for the **emp** table and bind the redaction function to **card_no**, **card_string**, and **salary** columns, respectively.

   .. code-block::

      CREATE REDACTION POLICY mask_emp ON emp WHEN (current_user IN ('matu', 'july'))
      ADD COLUMN card_no WITH mask_full(card_no),
      ADD COLUMN card_string WITH mask_partial(card_string, 'VVVVFVVVVFVVVVFVVVV','VVVV-VVVV-VVVV-VVVV','#',1,12),
      ADD COLUMN salary WITH mask_partial(salary, '9', 1, length(salary) - 2);

#. Switch to users **matu** and **july** and view the **emp** table, respectively.

   .. code-block::

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

#. User **matu** also has the permission for viewing all employee information, but user **july** does not. Modify the effective scope of the redaction policy.

   .. code-block::

      ALTER REDACTION POLICY mask_emp ON emp WHEN(current_user = 'july');

#. Switch to users **matu** and **july** and view the **emp** table again, respectively.

   .. code-block::

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

#. The information in the **phone_no**, **email**, and **birthday** columns is private data. Update the redaction policy **mask_emp** and add three redacted columns.

   .. code-block::

      ALTER REDACTION POLICY mask_emp ON emp ADD COLUMN phone_no WITH mask_partial(phone_no, '*', 4);
      ALTER REDACTION POLICY mask_emp ON emp ADD COLUMN email WITH mask_partial(email, '*', 1, position('@' in email));
      ALTER REDACTION POLICY mask_emp ON emp ADD COLUMN birthday WITH mask_full(birthday);

#. Switch to user **july** and view the **emp** table data.

   .. code-block::

      SET ROLE july PASSWORD 'password';
      SELECT * FROM emp;
       id | name |  phone_no   | card_no |     card_string     |        email         |   salary   |      birthday
      ----+------+-------------+---------+---------------------+----------------------+------------+---------------------
        1 | anny | 134******** |       0 | ####-####-####-1234 | ********163.com      | 99999.9990 | 1970-01-01 00:00:00
        2 | bob  | 182******** |       0 | ####-####-####-3456 | ***********qq.com    |  9999.9990 | 1970-01-01 00:00:00
        3 | cici | 155******** |         |                     | ************sina.com |            | 1970-01-01 00:00:00
      (3 rows)

#. Query **redaction_policies** and **redaction_columns** to view details about the current redaction policy **mask_emp**.

   .. code-block::

      SELECT * FROM redaction_policies;
       object_schema | object_owner | object_name | policy_name |            expression             | enable | policy_description
      ---------------+--------------+-------------+-------------+-----------------------------------+--------+--------------------
       public        | alice        | emp         | mask_emp    | ("current_user"() = 'july'::name) | t      |
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

#. Add the **salary_info** column. To replace the salary information in text format with \*.*, you can create a user-defined redaction function. In this step, you can use the PL/pgSQL to define the redaction function **mask_regexp_salary**. To create a redaction column, you simply need to customize the function name and parameter list. For details, see :ref:`User-Defined Functions <dws_04_0507>`.

   .. code-block::

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

#. If there is no need to set a redaction policy for the **emp** table, delete the redaction policy **mask_emp**.

   .. code-block::

      DROP REDACTION POLICY mask_emp ON emp;
