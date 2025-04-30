:original_name: dws_04_0851.html

.. _dws_04_0851:

PV_SESSION_MEMORY
=================

**PV_SESSION_MEMORY** displays statistics about memory usage at the session level in the unit of MB, including all the memory allocated to Postgres and Stream threads on DNs for jobs currently executed by users.

.. table:: **Table 1** PV_SESSION_MEMORY columns

   +----------+---------+---------------------------------------------------------------------------------------------+
   | Column   | Type    | Description                                                                                 |
   +==========+=========+=============================================================================================+
   | sessid   | Text    | Thread start time and ID.                                                                   |
   +----------+---------+---------------------------------------------------------------------------------------------+
   | init_mem | Integer | Memory allocated to the currently executed task before the task enters the executor, in MB. |
   +----------+---------+---------------------------------------------------------------------------------------------+
   | used_mem | Integer | Memory allocated to the currently executed task, in MB.                                     |
   +----------+---------+---------------------------------------------------------------------------------------------+
   | peak_mem | Integer | Peak memory allocated to the currently executed task, in MB.                                |
   +----------+---------+---------------------------------------------------------------------------------------------+
