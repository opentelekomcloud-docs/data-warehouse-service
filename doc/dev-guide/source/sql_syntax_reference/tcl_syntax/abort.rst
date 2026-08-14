:original_name: dws_06_0256.html

.. _dws_06_0256:

ABORT
=====

Function
--------

**ABORT** rolls back the current transaction and cancels the changes in the transaction.

This command is equivalent to :ref:`ROLLBACK <dws_06_0266>`, and is present only for historical reasons. Now **ROLLBACK** is recommended.

For more information about transaction management, see :ref:`Transaction Management <dws_06_0117>`.

Precautions
-----------

**ABORT** has no impact outside a transaction, but will provoke a warning.

Syntax
------

::

   ABORT [ WORK | TRANSACTION ] ;

Parameter Description
---------------------

**WORK \| TRANSACTION**

This keyword is optional and does not affect the **ABORT** operation.

Examples
--------

The following demonstrates how to use the **ABORT** statement to roll back and undo the process of modifying an account balance.

#. Create a test table and import data.

   ::

      -- Create a test table.
      DROP TABLE IF EXISTS user_account;
      CREATE TABLE  user_account (
          id INT PRIMARY KEY,
          username VARCHAR(20),
          balance DECIMAL(10,2)
      );
      -- Insert initial data.
      INSERT INTO user_account VALUES (1, 'lily', 100.00);
      INSERT INTO user_account VALUES (2, 'lilei', 200.00);
      -- View initial data.
      SELECT * FROM user_account;

   |image1|

#. Use **ABORT** to roll back within a transaction.

   a. Start a transaction.

      ::

         BEGIN TRANSACTION;

   b. Execute a modification operation (deduct Lily's balance).

      ::

         UPDATE user_account SET balance = balance - 50 WHERE id = 1;

   c. View the modified data (temporarily effective within the transaction).

      ::

         SELECT * FROM user_account;

      |image2|

   d. Execute **ABORT** to roll back (undo all modifications).

      ::

         ABORT;

      There are also equivalent syntax forms, such as **ABORT WORK** or **ABORT TRANSACTION**. The modern syntax uses **ROLLBACK**.

   e. Verify the rollback result: The data is restored to its initial values.

      ::

         SELECT * FROM user_account;

      |image3|

Helpful Links
-------------

:ref:`Transaction Management <dws_06_0117>`, :ref:`SET TRANSACTION <dws_06_0264>`, :ref:`COMMIT | END <dws_06_0259>`, and :ref:`ROLLBACK <dws_06_0266>`

.. |image1| image:: /_static/images/en-us_image_0000002567737449.png
.. |image2| image:: /_static/images/en-us_image_0000002536904528.png
.. |image3| image:: /_static/images/en-us_image_0000002536746518.png
