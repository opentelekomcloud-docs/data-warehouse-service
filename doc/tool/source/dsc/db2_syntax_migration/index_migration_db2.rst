:original_name: dws_07_6843.html

.. _dws_07_6843:

Index Migration (DB2)
=====================

Reverse Scans
-------------

Reverse Scans

+----------------------------------------+--------------------------------------------+
| DB2 Syntax                             | Syntax After Migration                     |
+========================================+============================================+
| .. code-block::                        | .. code-block::                            |
|                                        |                                            |
|    CREATE INDEX IDX_1 ON EMP           |    CREATE INDEX IDX_1 ON EMP               |
|       (ID ASC) ALLOW REVERSE SCANS;    |       (ID ASC) /*ALLOW REVERSE SCANS*/;    |
|     CREATE INDEX IDX_1 ON EMP          |     CREATE INDEX IDX_1 ON EMP              |
|       (ID ASC) DISALLOW REVERSE SCANS; |       (ID ASC) /*DISALLOW REVERSE SCANS*/; |
+----------------------------------------+--------------------------------------------+

Schema
------

index's schema is different from its table's schema.

+----------------------------------------------+----------------------------------------+
| DB2 Syntax                                   | Syntax After Migration                 |
+==============================================+========================================+
| .. code-block::                              | .. code-block::                        |
|                                              |                                        |
|    CREATE INDEX IDXSC.IDX_1 ON EMP (ID ASC); |    CREATE INDEX IDX_1 ON EMP (ID ASC); |
+----------------------------------------------+----------------------------------------+
