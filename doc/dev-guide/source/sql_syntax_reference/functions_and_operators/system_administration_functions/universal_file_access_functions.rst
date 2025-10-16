:original_name: dws_06_0054.html

.. _dws_06_0054:

Universal File Access Functions
===============================

Universal file access functions provide local access interfaces for files on a database server. Only files in the database cluster directory and the **log_directory** directory can be accessed. Use a relative path for files in the cluster directory, and a path matching the **log_directory** configuration setting for log files. Only database system administrators can use these functions.

pg_ls_dir(dirname text)
-----------------------

Description: Lists files in a directory.

Return type: setof text

Note: **pg_ls_dir** returns all the names in the specified directory, except the special entries "." and "..".

Examples:

::

   SELECT pg_ls_dir('./');
         pg_ls_dir
   ----------------------
    .postgresql.conf.swp
    postgresql.conf
    pg_tblspc
    PG_VERSION
    pg_ident.conf
    core
    server.crt
    pg_serial
    pg_twophase
    postgresql.conf.lock
    pg_stat_tmp
    pg_notify
    pg_subtrans
    pg_ctl.lock
    pg_xlog
    pg_clog
    base
    pg_snapshots
    postmaster.opts
    postmaster.pid
    server.key.rand
    server.key.cipher
    pg_multixact
    pg_errorinfo
    server.key
    pg_hba.conf
    pg_replslot
    .pg_hba.conf.swp
    cacert.pem
    pg_hba.conf.lock
    global
    gaussdb.state
   (32 rows)

pg_read_file(filename text, offset bigint, length bigint)
---------------------------------------------------------

Description: Returns the content of a text file.

Return type: text

Note: **pg_read_file** returns part of a text file. It can return a maximum of *length* bytes from *offset*. The actual size of fetched data is less than *length* if the end of the file is reached first. If **offset** is negative, it is the length rolled back from the file end. If **offset** and **length** are omitted, the entire file is returned.

Examples:

::

   SELECT pg_read_file('postmaster.pid',0,100);
                pg_read_file
   ---------------------------------------
    53078                                +
    /srv/BigData/hadoop/data1/coordinator+
    1500022474                           +
    8000                                 +
    /var/run/dws                         +
    localhost                            +
     2
   (1 row)

pg_read_binary_file(filename text [, offset bigint, length bigint,missing_ok boolean])
--------------------------------------------------------------------------------------

Description: Returns the content of a binary file.

Return type: bytea

Note: **pg_read_binary_file** is similar to **pg_read_file**, except that the result is a **bytea** value; accordingly, no encoding checks are performed. In combination with the **convert_from** function, this function can be used to read a file in a specified encoding:

::

   SELECT convert_from(pg_read_binary_file('filename'), 'UTF8');

pg_stat_file(filename text)
---------------------------

Description: Returns status information about a file.

Return type: record

Note: **pg_stat_file** returns a record containing the file size, last access timestamp, last modification timestamp, last file status change timestamp, and a **boolean** value indicating if it is a directory. Typical use cases are as follows:

::

   SELECT * FROM pg_stat_file('filename');

::

   SELECT (pg_stat_file('filename')).modification;

Examples:

::

   SELECT * FROM pg_stat_file('postmaster.pid');

    size |         access         |      modification      |         change
   | creation | isdir
   ------+------------------------+------------------------+------------------------
   +----------+-------
     117 | 2017-06-05 11:06:34+08 | 2017-06-01 17:18:08+08 | 2017-06-01 17:18:08+08
   |          | f
   (1 row)

::

   SELECT (pg_stat_file('postmaster.pid')).modification;
         modification
   ------------------------
    2017-06-01 17:18:08+08
   (1 row)
