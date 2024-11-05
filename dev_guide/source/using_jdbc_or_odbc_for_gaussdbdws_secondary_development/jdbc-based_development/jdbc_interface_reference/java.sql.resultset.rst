:original_name: dws_04_0108.html

.. _dws_04_0108:

java.sql.ResultSet
==================

This section describes **java.sql.ResultSet**, the interface for execution result sets.

.. table:: **Table 1** Support status for java.sql.ResultSet

   ================================= =========== ==============
   Method Name                       Return Type Support JDBC 4
   ================================= =========== ==============
   findColumn(String columnLabel)    int         Yes
   getBigDecimal(int columnIndex)    BigDecimal  Yes
   getBigDecimal(String columnLabel) BigDecimal  Yes
   getBoolean(int columnIndex)       boolean     Yes
   getBoolean(String columnLabel)    boolean     Yes
   getByte(int columnIndex)          byte        Yes
   getBytes(int columnIndex)         byte[]      Yes
   getByte(String columnLabel)       byte        Yes
   getBytes(String columnLabel)      byte[]      Yes
   getDate(int columnIndex)          Date        Yes
   getDate(String columnLabel)       Date        Yes
   getDouble(int columnIndex)        double      Yes
   getDouble(String columnLabel)     double      Yes
   getFloat(int columnIndex)         float       Yes
   getFloat(String columnLabel)      float       Yes
   getInt(int columnIndex)           int         Yes
   getInt(String columnLabel)        int         Yes
   getLong(int columnIndex)          long        Yes
   getLong(String columnLabel)       long        Yes
   getShort(int columnIndex)         short       Yes
   getShort(String columnLabel)      short       Yes
   getString(int columnIndex)        String      Yes
   getString(String columnLabel)     String      Yes
   getTime(int columnIndex)          Time        Yes
   getTime(String columnLabel)       Time        Yes
   getTimestamp(int columnIndex)     Timestamp   Yes
   getTimestamp(String columnLabel)  Timestamp   Yes
   isAfterLast()                     boolean     Yes
   isBeforeFirst()                   boolean     Yes
   isFirst()                         boolean     Yes
   next()                            boolean     Yes
   ================================= =========== ==============

.. note::

   -  One Statement cannot have multiple open ResultSets.
   -  The cursor that is used for traversing the ResultSet cannot be open after committed.
