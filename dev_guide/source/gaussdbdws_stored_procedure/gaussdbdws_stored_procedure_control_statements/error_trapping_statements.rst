:original_name: dws_04_0540.html

.. _dws_04_0540:

Error Trapping Statements
=========================

By default, any error occurring in a PL/SQL function aborts execution of the function, and indeed of the surrounding transaction as well. You can trap errors and restore from them by using a **BEGIN** block with an **EXCEPTION** clause. The syntax is an extension of the normal syntax for a **BEGIN** block:

::

   [<<label>>]
   [DECLARE
       declarations]
   BEGIN
       statements
   EXCEPTION
       WHEN condition [OR condition ...] THEN
           handler_statements
       [WHEN condition [OR condition ...] THEN
           handler_statements
       ...]
   END;

If no error occurs, this form of block simply executes all the statements, and then control passes to the next statement after **END**. But if an error occurs inside the executed statement, the statement rolls back and goes to the EXCEPTION list to find the first condition that matches the error. If a match is found, the corresponding **handler_statements** are executed, and then control passes to the next statement after **END**. If no match is found, the error propagates out as though the **EXCEPTION** clause were not there at all:

The error can be caught by an enclosing block with **EXCEPTION**, or if there is none it aborts processing of the function.

The *condition* can be any of those shown in SQL standard error codes. The special condition name **OTHERS** matches every error type except **QUERY_CANCELED**.

If a new error occurs within the selected **handler_statements**, it cannot be caught by this **EXCEPTION** clause, but is propagated out. A surrounding **EXCEPTION** clause could catch it.

When an error is caught by an **EXCEPTION** clause, the local variables of the PL/SQL function remain as they were when the error occurred, but all changes to persistent database state within the block are rolled back.

Example:

::

   CREATE TABLE mytab(id INT,firstname VARCHAR(20),lastname VARCHAR(20)) DISTRIBUTE BY hash(id);

   INSERT INTO mytab(firstname, lastname) VALUES('Tom', 'Jones');

   CREATE FUNCTION fun_exp() RETURNS INT
   AS $$
   DECLARE
       x INT :=0;
       y INT;
   BEGIN
       UPDATE mytab SET firstname = 'Joe' WHERE lastname = 'Jones';
       x := x + 1;
       y := x / 0;
   EXCEPTION
       WHEN division_by_zero THEN
           RAISE NOTICE 'caught division_by_zero';
           RETURN x;
   END;$$
   LANGUAGE plpgsql;

   CALL fun_exp();
   NOTICE:  caught division_by_zero
    fun_exp
   ---------
          1
   (1 row)

   SELECT * FROM mytab;
    id | firstname | lastname
   ----+-----------+----------
       | Tom       | Jones
   (1 row)

   DROP FUNCTION fun_exp();
   DROP TABLE mytab;

When control reaches the assignment to **y**, it will fail with a **division_by_zero** error. This will be caught by the **EXCEPTION** clause. The value returned in the **RETURN** statement will be the incremented value of **x**.

.. note::

   A block containing an **EXCEPTION** clause is more expensive to enter and exit than a block without one. Therefore, do not use **EXCEPTION** without need.

   In the following scenario, an exception cannot be caught, and the entire transaction rolls back. The threads of the nodes participating the stored procedure exit abnormally due to node failure and network fault, or the source data is inconsistent with that of the table structure of the target table during the COPY FROM operation.

Example: Exceptions with **UPDATE**/**INSERT**

This example uses exception handling to perform either **UPDATE** or **INSERT**, as appropriate:

::

   CREATE TABLE db (a INT, b TEXT);

   CREATE FUNCTION merge_db(key INT, data TEXT) RETURNS VOID AS
   $$
   BEGIN
       LOOP

   -- Try updating the key:
           UPDATE db SET b = data WHERE a = key;
           IF found THEN
               RETURN;
           END IF;
   -- Not there, so try to insert the key. If someone else inserts the same key concurrently, we could get a unique-key failure.
           BEGIN
               INSERT INTO db(a,b) VALUES (key, data);
               RETURN;
           EXCEPTION WHEN unique_violation THEN
           -- Loop to try the UPDATE again:
           END;
        END LOOP;
   END;
   $$
   LANGUAGE plpgsql;

   SELECT merge_db(1, 'david');
   SELECT merge_db(1, 'dennis');

   -- Delete FUNCTION and TABLE:
   DROP FUNCTION merge_db;
   DROP TABLE db ;
