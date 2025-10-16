:original_name: dws_06_0338.html

.. _dws_06_0338:

Session Information Functions
=============================

current_catalog
---------------

Description: name of the current database (called **catalog** in the SQL standard), same as **current_database()**.

Return type: name

Example:

::

   SELECT current_catalog;
    current_database
   ------------------
    gaussdb
   (1 row)

current_database()
------------------

Description: Name of the current database

Return type: name

Example:

::

   SELECT current_database();
    current_database
   ------------------
    gaussdb
   (1 row)

current_query()
---------------

Description: Text of the currently executing query, as submitted by the client (might contain more than one statement)

Return type: text

Example:

::

   SELECT current_query();
         current_query
   -------------------------
    SELECT current_query();
   (1 row)

current_schema[()]
------------------

Description: **current_schema** returns the first valid schema name in the search path. (If the search path is empty or contains no valid schema name, **NULL** is returned.) This is the schema that will be used for any tables or other named objects that are created without specifying a target schema.

Return type: name

Example:

::

   SELECT current_schema();
    current_schema
   ----------------
    public
   (1 row)

current_schemas(boolean)
------------------------

Description: **current_schemas(boolean)** returns an array of the names of all schemas presently in the search path. The Boolean option determines whether implicitly included system schemas such as **pg_catalog** are included in the returned search path.

Return type: name[]

Example:

::

   SELECT current_schemas(true);
      current_schemas
   ---------------------
    {pg_catalog,public}
   (1 row)

.. note::

   The search path can be altered at run time. The command is:

   ::

      SET search_path TO schema [, schema, ...]

current_user
------------

Description: Username of current execution context. **current_user** is the identifier of the user whose permission needs to be checked. It is usually used to represent a session user, but this setting can be changed according to :ref:`SET ROLE <dws_06_0222>`. It also changes during the execution of functions with the attribute **SECURITY DEFINER**.

Return type: name

Example:

::

   SELECT current_user;
    current_user
   --------------
    dbadmin
   (1 row)

inet_client_addr()
------------------

Description: Displays the IP address of the currently connected client.

.. note::

   -  It is available only in remote connection mode.
   -  If the database is connected to the local PC, the value is empty.

Return type: inet

Example:

::

   SELECT inet_client_addr();
    inet_client_addr
   ------------------
    10.10.0.50
   (1 row)

inet_client_port()
------------------

Description: Displays the port number of the currently connected client.

.. note::

   It is available only in remote connection mode.

Return type: integer

Example:

::

   SELECT inet_client_port();
    inet_client_port
   ------------------
               33143
   (1 row)

inet_server_addr()
------------------

Description: Displays the IP address of the current server.

.. note::

   -  It is available only in remote connection mode.
   -  If the database is connected to the local PC, the value is empty.

Return type: inet

Example:

::

   SELECT inet_server_addr();
    inet_server_addr
   ------------------
    10.10.0.13
   (1 row)

inet_server_port()
------------------

Description: Displays the port of the current server. All these functions return NULL if the current connection is via a Unix-domain socket.

.. note::

   It is available only in remote connection mode.

Return type: integer

Example:

::

   SELECT inet_server_port();
    inet_server_port
   ------------------
    8000
   (1 row)

pg_backend_pid()
----------------

Description: Process ID of the server process attached to the current session

Return type: integer

Example:

::

   SELECT pg_backend_pid();
    pg_backend_pid
   -----------------
    140229352617744
   (1 row)

pg_conf_load_time()
-------------------

Description: Configures load time. **pg_conf_load_time** returns the timestamp with time zone when the server configuration files were last loaded.

Return type: timestamp with time zone

Example:

::

   SELECT pg_conf_load_time();
         pg_conf_load_time
   ------------------------------
    2017-09-01 16:05:23.89868+08
   (1 row)

pg_my_temp_schema()
-------------------

Description: **pg_my_temp_schema** returns the OID of the current session's temporary schema, or zero if it has none (because it has not created any temporary tables). **pg_is_other_temp_schema** returns true if the given OID is the OID of another session's temporary schema.

Return type: OID

Example:

::

   SELECT pg_my_temp_schema();
    pg_my_temp_schema
   -------------------
                    0
   (1 row)

pg_is_other_temp_schema(oid)
----------------------------

Description: Whether the schema is the temporary schema of another session.

Return type: boolean

Example:

::

   SELECT pg_is_other_temp_schema(25356);
    pg_is_other_temp_schema
   -------------------------
    f
   (1 row)

pg_postmaster_start_time()
--------------------------

Description: Specifies the start time of the database instance. **pg_postmaster_start_time** returns the timestamp (with time zone) when the database instance was started.

Return type: timestamp with time zone

Example:

::

   SELECT pg_postmaster_start_time();
      pg_postmaster_start_time
   ------------------------------
    2017-08-30 16:02:54.99854+08
   (1 row)

pg_trigger_depth()
------------------

Description: Current nesting level of triggers

Return type: integer

Example:

::

   SELECT pg_trigger_depth();
    pg_trigger_depth
   ------------------
                   0
   (1 row)

pgxc_version()
--------------

Description: Postgres-XC version information

Return type: text

Example:

::

   SELECT pgxc_version();
                                                   pgxc_version
   -------------------------------------------------------------------------------------------------------------
    Postgres-XC 1.1 on x86_64-unknown-linux-gnu, based on PostgreSQL 9.2.4, compiled by g++ (GCC) 5.4.0, 64-bit
   (1 row)

session_user
------------

Description: Session user name **session_user** is usually the user who initiated the current database connection, but administrators can change this setting with :ref:`SET SESSION AUTHORIZATION <dws_06_0223>`.

Return type: name

Example:

::

   SELECT session_user;
    session_user
   --------------
    dbadmin
   (1 row)

user
----

Description: Is equivalent to **current_user**.

Return type: name

Example:

::

   SELECT user;
    current_user
   --------------
    dbadmin
   (1 row)

version()
---------

Description: version information. **version** returns a string describing a server's version.

Return type: text

Example:

::

   SELECT version();
                                                                   version
   ---------------------------------------------------------------------------------------------------------------------------------------
    PostgreSQL 9.2.4 gsql ((GaussDB 8.2.1 build 39137c2d) compiled at 2022-09-23 15:43:11 commit 3629 last mr 5138 release) on x86_64-unknown-linux-gnu, compiled by g++ (GCC) 5.4.0, 64-bit
   (1 row)
