:original_name: dws_04_0116.html

.. _dws_04_0116:

CopyManager
===========

CopyManager is an API interface class provided by the JDBC driver in GaussDB(DWS). It is used to import data to GaussDB(DWS) in batches.

Inheritance Relationship of CopyManager
---------------------------------------

The CopyManager class is in the **org.postgresql.copy** package class and inherits the java.lang.Object class. The declaration of the class is as follows:

.. code-block::

   public class CopyManager
   extends Object

Construction Method
-------------------

public CopyManager(BaseConnection connection)

throws SQLException

Basic Methods
-------------

.. table:: **Table 1** Common methods of CopyManager

   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | Return Value | Method                                               | Description                                                                               | throws                   |
   +==============+======================================================+===========================================================================================+==========================+
   | CopyIn       | copyIn(String sql)                                   | ``-``                                                                                     | SQLException             |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long         | copyIn(String sql, InputStream from)                 | Uses **COPY FROM STDIN** to quickly load data to tables in the database from InputStream. | SQLException,IOException |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long         | copyIn(String sql, InputStream from, int bufferSize) | Uses **COPY FROM STDIN** to quickly load data to tables in the database from InputStream. | SQLException,IOException |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long         | copyIn(String sql, Reader from)                      | Uses **COPY FROM STDIN** to quickly load data to tables in the database from Reader.      | SQLException,IOException |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long         | copyIn(String sql, Reader from, int bufferSize)      | Uses **COPY FROM STDIN** to quickly load data to tables in the database from Reader.      | SQLException,IOException |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | CopyOut      | copyOut(String sql)                                  | ``-``                                                                                     | SQLException             |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long         | copyOut(String sql, OutputStream to)                 | Sends the result set of **COPY TO STDOUT** from the database to the OutputStream class.   | SQLException,IOException |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long         | copyOut(String sql, Writer to)                       | Sends the result set of **COPY TO STDOUT** from the database to the Writer class.         | SQLException,IOException |
   +--------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
