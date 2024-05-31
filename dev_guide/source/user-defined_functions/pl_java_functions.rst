:original_name: dws_04_0509.html

.. _dws_04_0509:

PL/Java Functions
=================

With the GaussDB(DWS) PL/Java functions, you can choose your favorite Java IDE to write Java methods and install the JAR files containing these methods into the GaussDB(DWS) database before invoking them. GaussDB(DWS) PL/Java is developed based on open-source PL/Java 1.5.5 and uses JRE 1.8.0_322.

Constraints
-----------

Java UDF can be used for some Java logical computing. You are not advised to encapsulate services in Java UDF.

-  You are not advised to connect to a database in any way (for example, JDBC) in Java functions.
-  Currently, only data types listed in :ref:`Table 1 <en-us_topic_0000001188482166__table10200627143416>` are supported. Other data types, such as user-defined data types and complex data types (for example, Java array and its derived types) are not supported.
-  Currently, UDAF and UDTF are not supported.

Examples
--------

Before using PL/Java, you need to pack the implementation of Java methods into a JAR package and deploy it into the database. Then, create functions as a database administrator. For compatibility purposes, use JRE 1.8.0_322 for compilation.

#. .. _en-us_topic_0000001188482166__li84576364168:

   Compile a JAR package.

   Java method implementation and JAR package archiving can be achieved in an integrated development environment (IDE). The following is a simple example of compilation and archiving through command lines. You can create a JAR package that contains a single method in the similar way.

   First, prepare an **Example.java** file that contains a method for converting substrings to uppercase. In the following example, **Example** is the class name and **upperString** is the method name:

   ::

      public class Example
      {
          public static String upperString (String text, int beginIndex, int endIndex)
          {
              return text.substring(beginIndex, endIndex).toUpperCase();
          }
      }

   Then, create a **manifest.txt** file containing the following content:

   ::

      Manifest-Version: 1.0
      Main-Class: Example
      Specification-Title: "Example"
      Specification-Version: "1.0"
      Created-By: 1.6.0_35-b10-428-11M3811
      Build-Date: 08/14/2018 10:09 AM

   **Manifest-Version** specifies the version of the **manifest** file. **Main-Class** specifies the main class used by the .jar file. **Specification-Title** and **Specification-Version** are the extended attributes of the package. **Specification-Title** specifies the title of the extended specification and **Specification-Version** specifies the version of the extended specification. **Created-By** specifies the person who created the file. **Build-Date** specifies the date when the file was created.

   Finally, archive the .java file and package it into **javaudf-example.jar**.

   ::

      javac Example.java
      jar cfm javaudf-example.jar manifest.txt Example.class

   .. important::

      JAR package names must comply with JDK rules. If a name contains invalid characters, an error occurs when a function is deployed or used.

#. Deploy the JAR package.

   Place the JAR package on the OBS server using the method described in For details, see "Uploading a File" in *Object Storage Service Console Operation Guide*.. Then, create the AK/SK. For details about how to obtain the AK/SK, see section :ref:`Creating Access Keys (AK and SK) <dws_04_0183>`. Log in to the database and run the **gs_extend_library** function to import the file to GaussDB(DWS).

   ::

      SELECT gs_extend_library('addjar', 'obs://bucket/path/javaudf-example.jar accesskey=access_key_value_to_be_replaced  secretkey=secret_access_key_value_to_be_replaced  region=region_name libraryname=example');

   For details about how to use the **gs_extend_library** function, see :ref:`Manage JAR packages and files <en-us_topic_0000001188482166__li1437422919465>`. Change the values of AK and SK as needed. Replace *region_name* with an actual region name.

#. Use a PL/Java function.

   Log in to the database as a user who has the **sysadmin** permission (for example, dbadmin) and create the **java_upperstring** function:

   ::

      CREATE FUNCTION java_upperstring(VARCHAR, INTEGER, INTEGER)
          RETURNS VARCHAR
          AS 'Example.upperString'
      LANGUAGE JAVA;

   .. note::

      -  The data type defined in the java_upperstring function should be a type in GaussDB(DWS) and match the data type defined in :ref:`1 <en-us_topic_0000001188482166__li84576364168>` in the upperString method in Java. For details about the mapping between GaussDB(DWS) and Java data types, see :ref:`Table 1 <en-us_topic_0000001188482166__table10200627143416>`.
      -  The AS clause specifies the class name and static method name of the Java method invoked by the function. The format is *Class name*\ **.**\ *Method name*. The class name and method name must match the Java class and method defined in :ref:`1 <en-us_topic_0000001188482166__li84576364168>`.
      -  To use PL/Java functions, set **LANGUAGE** to **JAVA**.
      -  For details about CREATE FUNCTION, see :ref:`Create functions <en-us_topic_0000001188482166__li1541715862915>`.

   Execute the java_upperstring function.

   ::

      SELECT java_upperstring('test', 0, 1);

   The expected result is as follows:

   ::

       java_upperstring
      ---------------------
       T
      (1 row)

#. Authorize a common user to use the PL/Java function.

   Create a common user named **udf_user**.

   ::

      CREATE USER udf_user PASSWORD 'password';

   This command grants user **udf_user** the permission for the java_upperstring function. Note that the user can use this function only if it also has the permission for using the schema of the function.

   ::

      GRANT ALL PRIVILEGES ON SCHEMA public TO udf_user;
      GRANT ALL PRIVILEGES ON FUNCTION java_upperstring(VARCHAR, INTEGER, INTEGER) TO udf_user;

   Log in to the database as user **udf_user**.

   ::

      SET SESSION SESSION AUTHORIZATION udf_user PASSWORD 'password';

   Execute the java_upperstring function.

   ::

      SELECT public.java_upperstring('test', 0, 1);

   The expected result is as follows:

   ::

       java_upperstring
      ---------------------
       T
      (1 row)

#. Delete the function.

   If you no longer need this function, delete it.

   ::

      DROP FUNCTION java_upperstring;

#. Uninstall the JAR package.

   Use the gs_extend_library function to uninstall the JAR package.

   ::

      SELECT gs_extend_library('rmjar', 'libraryname=example');

SQL Definition and Usage
------------------------

-  .. _en-us_topic_0000001188482166__li1437422919465:

   **Manage JAR packages and files.**

   A database user having the **sysadmin** permission can use the gs_extend_library function to deploy, view, and delete JAR packages in the database. The syntax of the function is as follows:

   ::

      SELECT gs_extend_library('[action]', '[operation]');

   .. note::

      -  **action**: operation action. The options are as follows:

         -  **ls**: Displays JAR packages in the database and checks the MD5 value consistency of files on each node.
         -  **addjar**: deploys a JAR package on the OBS server in the database.
         -  **rmjar**: Deletes JAR packages from the database.

      -  **operation**: operation string. The format can be either of the following:

         obs://[bucket]/[source_filepath] accesskey=[accesskey] secretkey=[secretkey] region=[region] libraryname=[libraryname]

         -  **bucket**: name of the bucket to which the OBS file belongs. It is mandatory.
         -  **source_filepath**: file path on the OBS server. Only .jar files are supported.
         -  **accesskey**: key obtained for accessing the OBS service. It is mandatory.
         -  **secret_key**: secret key obtained for the OBS service. It is mandatory.
         -  **region**: region where the OBS bucket stored in the JAR package of a user-defined function belongs to. This parameter is mandatory.
         -  **libraryname**: user-defined library name, which is used to invoke JAR files in GaussDB(DWS). If **action** is set to **addjar** or **rmjar**, **libraryname** must be specified. If **action** is set to **ls**, **libraryname** is optional. Note that a user-defined library name cannot contain the following characters: ``/|;&$<>\'{}"()[]~*?!``

-  .. _en-us_topic_0000001188482166__li1541715862915:

   Create functions.

   PL/Java functions can be created using the **CREATE FUNCTION** syntax and are defined as **LANGUAGE JAVA**, including the **RETURNS** and **AS** clauses.

   -  To use **CREATE FUNCTION**, specify the name and parameter type for the function to be created.

   -  The **RETURNS** clause specifies the return type for the function.

   -  The **AS** clause specifies the class name and static method name of the Java method to be invoked. If the **NULL** value needs to be transferred to the Java method as an input parameter, specify the name of the Java encapsulation class corresponding to the parameter type. For details, see :ref:`NULL Handling <en-us_topic_0000001188482166__section11546180328>`.

   -  For details about the syntax, see CREATE FUNCTION.

      ::

         CREATE [ OR REPLACE ] FUNCTION function_name
         ( [ { argname [ argmode ] argtype [ { DEFAULT | := | = } expression ]} [, ...] ])
         [ RETURNS rettype [ DETERMINISTIC ] ]
         LANGAUGE JAVA
         [
             { IMMUTABLE | STATBLE | VOLATILE }
             | [ NOT ] LEAKPROOF
             | WINDOW
             | { CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT |STRICT }
             | {[ EXTERNAL ] SECURITY INVOKER | [ EXTERNAL ] SECURITY DEFINER | AUTHID DEFINER | AUTHID CURRENT_USER}
             | { FENCED }
             | COST execution_cost
             | ROWS result_rows
             | SET configuration_parameter { {TO |=} value | FROM CURRENT}
         ] [...]
         {
             AS 'class_name.method_name' ( { argtype } [, ...] )
         }

-  Use functions.

   During execution, PL/Java searches for the Java class specified by a function among all the deployed JAR packages, which are ranked by name in alphabetical order, invokes the Java method in the first found class, and returns results.

-  Delete functions.

   PL/Java functions can be deleted by using the **DROP FUNCTION** syntax. For details about the syntax, see DROP FUNCTION.

   .. code-block::

      DROP FUNCTION [ IF EXISTS ] function_name [ ( [ {[ argmode ] [ argname ] argtype} [, ...] ] ) [ CASCADE | RESTRICT ] ];

   To delete an overloaded function (for details, see :ref:`Overloaded Functions <en-us_topic_0000001188482166__section13355162616820>`), specify **argtype** in the function. To delete other functions, simply specify **function_name**.

-  .. _en-us_topic_0000001188482166__li19929482465:

   Authorize permissions for functions.

   Only user **sysadmin** can create PL/Java functions. It can also grant other users the permission to use the PL/Java functions. For details about the syntax, see GRANT.

   .. code-block::

      GRANT { EXECUTE | ALL [ PRIVILEGES ] }
          ON { FUNCTION {function_name ( [ {[ argmode ] [ arg_name ] arg_type} [, ...] ] )} [, ...]
              | ALL FUNCTIONS IN SCHEMA schema_name [, ...] }
          TO { [ GROUP ] role_name | PUBLIC } [, ...]
          [ WITH GRANT OPTION ];

Mapping for Basic Data Types
----------------------------

.. _en-us_topic_0000001188482166__table10200627143416:

.. table:: **Table 1** PL/Java mapping for default data types

   ============ ==================================================
   GaussDB(DWS) Java
   ============ ==================================================
   BOOLEAN      boolean
   "char"       byte
   bytea        byte[]
   SMALLINT     short
   INTEGER      int
   BIGINT       long
   FLOAT4       float
   FLOAT8       double
   CHAR         java.lang.String
   VARCHAR      java.lang.String
   TEXT         java.lang.String
   name         java.lang.String
   DATE         java.sql.Timestamp
   TIME         java.sql.Time (stored value treated as local time)
   TIMETZ       java.sql.Time
   TIMESTAMP    java.sql.Timestamp
   TIMESTAMPTZ  java.sql.Timestamp
   ============ ==================================================

Array Type Processing
---------------------

GaussDB(DWS) can convert basic array types. You only need to append a pair of square brackets ([]) to the data type when creating a function.

.. code-block::

   CREATE FUNCTION java_arrayLength(INTEGER[])
       RETURNS INTEGER
       AS 'Example.getArrayLength'
   LANGUAGE JAVA;

Java code is similar to the following:

.. code-block::

   public class Example
   {
       public static int getArrayLength(Integer[] intArray)
       {
           return intArray.length;
       }
   }

Invoke the following statement:

.. code-block::

   SELECT java_arrayLength(ARRAY[1, 2, 3]);

The expected result is as follows:

.. code-block::

   java_arrayLength
   ---------------------
   3
   (1 row)

.. _en-us_topic_0000001188482166__section11546180328:

NULL Handling
-------------

NULL values cannot be handled for GaussDB(DWS) data types that are mapped and can be converted to simple Java types by default. If you use a Java function to obtain and process the **NULL** value transferred from GaussDB(DWS), specify the Java encapsulation class in the **AS** clause as follows:

.. code-block::

   CREATE FUNCTION java_countnulls(INTEGER[])
       RETURNS INTEGER
       AS 'Example.countNulls(java.lang.Integer[])'
   LANGUAGE JAVA;

Java code is similar to the following:

.. code-block::

   public class Example
   {
       public static int countNulls(Integer[] intArray)
       {
           int nullCount = 0;
           for (int idx = 0; idx < intArray.length; ++idx)
           {
               if (intArray[idx] == null)
               nullCount++;
           }
           return nullCount;
       }
   }

Invoke the following statement:

.. code-block::

   SELECT java_countNulls(ARRAY[null, 1, null, 2, null]);

The expected result is as follows:

.. code-block::

   java_countNulls
   --------------------
   3
   (1 row)

.. _en-us_topic_0000001188482166__section13355162616820:

Overloaded Functions
--------------------

PL/Java supports overloaded functions. You can create functions with the same name or invoke overloaded functions from Java code. The procedure is as follows:

#. Create overloaded functions.

   For example, create two Java methods with the same name, and specify the methods dummy(int) and dummy(String) with different parameter types.

   .. code-block::

      public class Example
      {
          public static int dummy(int value)
          {
              return value*2;
          }
          public static String dummy(String value)
          {
              return value;
          }
      }

   In addition, create two functions with the same names as the above two functions in GaussDB(DWS).

   .. code-block::

      CREATE FUNCTION java_dummy(INTEGER)
          RETURNS INTEGER
          AS 'Example.dummy'
      LANGUAGE JAVA;

      CREATE FUNCTION java_dummy(VARCHAR)
          RETURNS VARCHAR
          AS 'Example.dummy'
      LANGUAGE JAVA;

#. Invoke the overloaded functions.

   GaussDB(DWS) invokes the functions that match the specified parameter type. The results of invoking the above two functions are as follows:

   .. code-block::

      SELECT java_dummy(5);
       java_dummy
      -----------------
                  10
      (1 row)

      SELECT java_dummy('5');
       java_dummy
      ---------------
      5
      (1 row)

   Note that GaussDB(DWS) may implicitly convert data types. Therefore, you are advised to specify the parameter type when invoking an overloaded function.

   .. code-block::

      SELECT java_dummy(5::varchar);
       java_dummy
      ----------------
      5
      (1 row)

   In this case, the specified parameter type is preferentially used for matching. If there is no Java method matching the specified parameter type, the system implicitly converts the parameter and searches for Java methods based on the conversion result.

   .. code-block::

      SELECT java_dummy(5::INTEGER);
       java_dummy
      -----------------
      10
      (1 row)

      DROP FUNCTION java_dummy(INTEGER);

      SELECT java_dummy(5::INTEGER);
       java_dummy
      ----------------
      5
      (1 row)

   .. important::

      Data types supporting implicit conversion are as follows:

      -  **SMALLINT**: It can be converted to the **INTEGER** type by default.
      -  **SMALLINT** and **INTEGER**: They can be converted to the **BIGINT** type by default.
      -  **TINYINT**, **SMALLINT**, **INTEGER**, and **BIGINT**: They can be converted to the **BOOL** type by default.
      -  **CHAR**, **NAME**, **BIGINT**, **INTEGER**, **SMALLINT**, **TINYINT**, **RAW**, **FLOAT4**, **FLOAT8**, **BPCHAR**, **VARCHAR**, **NVARCHAR2**, **DATE**, **TIMESTAMP**, **TIMESTAMPTZ**, **NUMERIC**, and **SMALLDATETIME**: They can be converted to the **TEXT** type by default.
      -  **TEXT**, **CHAR**, **BIGINT**, **INTEGER**, **SMALLINT**, **TINYINT**, **RAW**, **FLOAT4**, **FLOAT8**, **BPCHAR**, **DATE**, **NVARCHAR2**, **TIMESTAMP**, **NUMERIC**, and **SMALLDATETIME**: They can be converted to the **VARCHAR** type by default.

#. Delete the overloaded functions.

   To delete an overloaded function, specify the parameter type for the function. Otherwise, the function cannot be deleted.

   .. code-block::

      DROP FUNCTION java_dummy(INTEGER);

GUC Parameters
--------------

-  **FencedUDFMemoryLimit**

   A session-level GUC parameter. It is used to specify the maximum virtual memory used by a single Fenced UDF Worker process initiated by a session.

   .. code-block::

      SET FencedUDFMemoryLimit='512MB';

   The value range of this parameter is (**150 MB**, **1G**). If the value is greater than **1G**, an error will be reported immediately. If the value is less than or equal to **150 MB**, an error will be reported during function invoking.

   .. important::

      -  If **FencedUDFMemoryLimit** is set to **0**, the virtual memory for a Fenced UDF Worker process will not be limited.
      -  You are advised to use **udf_memory_limit** to control the physical memory used by Fenced UDF Worker processes. You are not advised to use **FencedUDFMemoryLimit**, especially when Java UDFs are used. If you are clear about the impact of this parameter, set it based on the following information:

         -  After a C Fenced UDF Worker process is started, it will occupy about 200 MB virtual memory, and about 16 MB physical memory.
         -  After a Java Fenced UDF Worker process is started, it will occupy about 2.5 GB virtual memory, and about 50 MB physical memory.

Exception Handling
------------------

If there is an exception in a JVM, PL/Java will export JVM stack information during the exception to a client.

Logging
-------

PL/Java uses the standard Java Logger. Therefore, you can record logs as follows:

.. code-block::

   Logger.getAnonymousLogger().config( "Time is " + new
   Date(System.currentTimeMillis()));

An initialized Java Logger class is set to the **CONFIG** level by default, corresponding to the **LOG** level in GaussDB(DWS). In this case, log messages generated by Java Logger are all redirected to the GaussDB(DWS) backend. Then, the log messages are written into server logs or displayed on the user interface. MPPDB server logs record information at the **LOG**, **WARNING**, and **ERROR** levels. The SQL user interface displays logs at the **WARNING** and **ERROR** levels. The following table lists mapping between Java Logger levels and GaussDB(DWS) log levels.

.. table:: **Table 2** PL/Java log levels

   ======================= ==========================
   java.util.logging.Level GaussDB(DWS) **Log Level**
   ======================= ==========================
   **SERVER**              ERROR
   **WARNING**             WARNING
   **CONFIG**              LOG
   **INFO**                INFO
   **FINE**                DEBUG1
   **FINER**               DEBUG2
   **FINEST**              DEBUG3
   ======================= ==========================

You can change Java Logger levels. For example, if the Java Logger level is changed to **SEVERE** by the following Java code, log messages (**msg**) will not be recorded in GaussDB(DWS) logs during **WARNING** logging.

.. code-block::

   Logger log = Logger.getAnonymousLogger();
   Log.setLevel(Level.SEVERE);
   log.log(Level.WARNING, msg);

Security Issues
---------------

In GaussDB(DWS), PL/Java is an untrusted language. Only user **sysadmin** can create PL/Java functions. The user can grant other users the permission for using the PL/Java functions. For details, see :ref:`Authorize permissions for functions <en-us_topic_0000001188482166__li19929482465>`.

In addition, PL/Java controls user access to file systems, forbidding users from reading most system files, or writing, deleting, or executing any system files in Java methods.
