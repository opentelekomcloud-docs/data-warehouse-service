:original_name: dws_04_0135.html

.. _dws_04_0135:

Case: Type of Record in Non-real Table Can Not Be Shipped
=========================================================

Possible Cause
--------------

In 8.2.1.\ *x*, the UPDATE statements + WITH statements cannot be pushed down.

Case 1: UPDATE Statements Are Not Pushed Down
---------------------------------------------

#. Set the GUC parameter **enable_stream_ctescan** to disable ctescan in the stream plan before statement execution.

   ::

      SET enable_stream_ctescan = off

#. Reset the parameter after the service statement is executed.

   ::

      SET enable_stream_ctescan = on

#. Add a hint after the UPDATE keyword in the statement. The specific method is **UPDATE /*+ set global(enable_stream_ctescan off) \*/**.
