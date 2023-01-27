:original_name: DWS_DS_130.html

.. _DWS_DS_130:

Managing SQL Terminal Connections
=================================

Data Studio allows you to reuse an existing SQL Terminal connection or create a new SQL Terminal connection for execution plan and cost, visual explain plan, and operations in the resultset. By default, the SQL Terminal reuses the existing connection to perform these operations.

Use new connection when there are multiple queries queued for execution in existing connection as the queries are executed sequentially and there may be a delay. Always reuse existing connection while working on temp tables. Refer to the :ref:`Editing Temporary Tables <dws_ds_103>` section to edit temp tables.

Complete the steps to enable or disable SQL Terminal connection reuse:

#. Click |image1| to enable or disable SQL Terminal connection reuse.

   Refer to the :ref:`FAQs <en-us_topic_0000001098993156__en-us_topic_0185264547_li18661844113712>` section for the behavior of query execution with reuse and new connection.

   .. note::

      Use the existing SQL Terminal connection to edit temporary tables.

.. |image1| image:: /_static/images/en-us_image_0000001098993266.png
