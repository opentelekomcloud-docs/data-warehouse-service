:original_name: dws_04_0585.html

.. _dws_04_0585:

PG_DEPEND
=========

**PG_DEPEND** records the dependency relationships between database objects. This information allows **DROP** commands to find which other objects must be dropped by **DROP CASCADE** or prevent dropping in the **DROP RESTRICT** case.

See also :ref:`PG_SHDEPEND <dws_04_0616>`, which provides similar functionality for recording dependencies between objects that are shared between database clusters.

.. table:: **Table 1** PG_DEPEND columns

   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name        | Type    | Reference                         | Description                                                                                                                                              |
   +=============+=========+===================================+==========================================================================================================================================================+
   | classid     | oid     | :ref:`PG_CLASS <dws_04_0578>`.oid | OID of the system catalog the dependent object is in                                                                                                     |
   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | objid       | oid     | Any OID column                    | OID of the specific dependent object                                                                                                                     |
   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | objsubid    | integer | ``-``                             | For a table column, this is the column number (the objid and classid refer to the table itself). For all other object types, this column is **0**.       |
   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | refclassid  | oid     | :ref:`PG_CLASS <dws_04_0578>`.oid | OID of the system catalog the referenced object is in                                                                                                    |
   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | refobjid    | oid     | Any OID column                    | OID of the specific referenced object                                                                                                                    |
   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | refobjsubid | integer | ``-``                             | For a table column, this is the column number (the refobjid and refclassid refer to the table itself). For all other object types, this column is **0**. |
   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
   | deptype     | "char"  | ``-``                             | A code defining the specific semantics of this dependency relationship                                                                                   |
   +-------------+---------+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+

In all cases, a **pg_depend** entry indicates that the referenced object cannot be dropped without also dropping the dependent object. However, there are several subflavors defined by **deptype**:

-  DEPENDENCY_NORMAL (n): A normal relationship between separately-created objects. The dependent object can be dropped without affecting the referenced object. The referenced object can only be dropped by specifying **CASCADE**, in which case the dependent object is dropped, too. Example: a table column has a normal dependency on its data type.
-  DEPENDENCY_AUTO (a): The dependent object can be dropped separately from the referenced object, and should be automatically dropped (regardless of RESTRICT or CASCADE mode) if the referenced object is dropped. Example: a named constraint on a table is made autodependent on the table, so that it will go away if the table is dropped.
-  DEPENDENCY_INTERNAL (i): The dependent object was created as part of creation of the referenced object, and is only a part of its internal implementation. A DROP of the dependent object will be disallowed outright (We'll tell the user to issue a DROP against the referenced object, instead). A DROP of the referenced object will be propagated through to drop the dependent object whether CASCADE is specified or not. For example, a trigger used to enforce a foreign key constraint is set to an item internally dependent on its constraint in :ref:`PG_CONSTRAINT <dws_04_0580>`.
-  DEPENDENCY_EXTENSION (e): The dependent object is a member of the extension that is the referenced object. (For details, see :ref:`PG_EXTENSION <dws_04_0589>`). The dependent object can be dropped via **DROP EXTENSION** on the referenced object. Functionally this dependency type acts the same as an internal dependency, but it is kept separate for clarity and to simplify **gs_dump**.
-  DEPENDENCY_PIN (p): There is no dependent object. This type of entry is a signal that the system itself depends on the referenced object, and so that object must never be deleted. Entries of this type are created only by **initdb**. The columns with dependent object are all zeroes.

Examples
--------

Query the table that depends on the database object sequence **serial1**:

#. Query the OID of the sequence **serial1** in the system catalog **PG_CLASS**.

   ::

      SELECT oid FROM pg_class WHERE relname ='serial1';
        oid
      -------
       17815
      (1 row)

#. Use the system catalog **PG_DEPEND** and the OID of **serial1** to obtain the objects that depend on **serial1**.

   ::

      SELECT * FROM pg_depend WHERE objid ='17815';
       classid | objid | objsubid | refclassid | refobjid | refobjsubid | deptype
      ---------+-------+----------+------------+----------+-------------+---------
          1259 | 17815 |        0 |       2615 |     2200 |           0 | n
          1259 | 17815 |        0 |       1259 |    17812 |           1 | a
      (2 rows)

#. Obtain the OID of the table that depends on the serial1 sequence based on the refobjid field and query the table name. The result indicates that the table **customer_address** depends on **serial1**.

   ::

      SELECT relname FROM pg_class where oid='17812';
           relname
      ------------------
       customer_address
      (1 row)
