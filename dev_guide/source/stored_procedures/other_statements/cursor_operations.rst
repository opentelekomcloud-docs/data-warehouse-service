:original_name: dws_04_0544.html

.. _dws_04_0544:

Cursor Operations
=================

GaussDB(DWS) provides cursors as a data buffer for users to store execution results of SQL statements. Each cursor region has a name. Users can use SQL statements to obtain records one by one from cursors and grant them to master variables, then being processed further by host languages.

Cursor operations include cursor definition, open, fetch, and close operations.

For the complete example of cursor operations, see :ref:`Explicit Cursor <dws_04_0547>`.
