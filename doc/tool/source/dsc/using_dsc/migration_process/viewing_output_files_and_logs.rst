:original_name: dws_16_0021.html

.. _dws_16_0021:

Viewing Output Files and Logs
=============================

Viewing and Verifying Output Files
----------------------------------

After the migration is complete, you can use a comparison tool (for example, BeyondCompare®) to compare the output file with its input file. Input SQL files can also be formatted for easier comparison.

#. Run the following command in Linux and view output files in the output folder. Operations in Windows are not described here.

   .. code-block::

      cd OUTPUT
      ls

   Information similar to the following is displayed:

   .. code-block::

      formattedSource    output
      user1@node79:~/Documentation/DSC/OUTPUT> cd output
      user1@node79:~/Documentation/DSC/OUTPUT/output> ls
      in_index.sql    input.sql    Input_table.sql    in_view.sql    MetadataInput.sql
      user1@node79:~/Documentation/DSC/OUTPUT/output>

2. Use the comparison tool to compare the output file with its input file. Check whether the keywords in the migrated SQL file meet the requirements of the target database. If not, contact technical support.

Viewing Log Files
-----------------

Execution information and error messages are written into corresponding log files. For details, see :ref:`Log Reference <en-us_topic_0000001813598960>`.

Check whether errors are logged. If they are, rectify the faults by following the instructions in :ref:`Troubleshooting <en-us_topic_0000001860318609>`.
