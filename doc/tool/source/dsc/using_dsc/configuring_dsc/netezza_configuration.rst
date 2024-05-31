:original_name: dws_16_0214.html

.. _dws_16_0214:

Netezza Configuration
=====================

Netezza parameters are used to customize rules for Netezza script migration.

Open the **features-netezza.properties** file in the **config** folder and set parameters in :ref:`Table 1 <en-us_topic_0000001819416089__en-us_topic_0000001706223977_en-us_topic_0218440629_table60771938143352>` as required.

.. _en-us_topic_0000001819416089__en-us_topic_0000001706223977_en-us_topic_0218440629_table60771938143352:

.. table:: **Table 1** Parameters in the **features-netezza.properties** file

   +-----------------------------------------+-------------------------------------------------------------+--------------+---------------+---------------------------------------------+
   | Parameter                               | Description                                                 | Value Range  | Default Value | Example                                     |
   +=========================================+=============================================================+==============+===============+=============================================+
   | -  rowstoreToColumnstore                | Whether to convert row-store tables to column-store tables. | -  true      | false         | rowstoreToColumnstore=false                 |
   |                                         |                                                             | -  false     |               |                                             |
   +-----------------------------------------+-------------------------------------------------------------+--------------+---------------+---------------------------------------------+
   | -  cstore_blob                          | The options are as follows:                                 | -  bytea     | bytea         | cstore_blob=bytea                           |
   |                                         |                                                             | -  none      |               |                                             |
   |                                         | -  bytea                                                    |              |               |                                             |
   |                                         | -  none                                                     |              |               |                                             |
   +-----------------------------------------+-------------------------------------------------------------+--------------+---------------+---------------------------------------------+
   | -  keywords_addressed_using_as          | The options are as follows:                                 | -  OWNER     | OWNER         | keywords_addressed_using_as=OWNER           |
   |                                         |                                                             | -  ATTRIBUTE |               |                                             |
   |                                         | -  OWNER                                                    | -  SOURCE    |               |                                             |
   |                                         | -  ATTRIBUTE                                                | -  FREEZE    |               |                                             |
   |                                         | -  SOURCE                                                   |              |               |                                             |
   |                                         | -  FREEZE                                                   |              |               |                                             |
   +-----------------------------------------+-------------------------------------------------------------+--------------+---------------+---------------------------------------------+
   | -  keywords_addressed_using_doublequote | Possible values of **addressed_using_doublequote**          | FREEZE       | FREEZE        | keywords_addressed_using_doublequote=FREEZE |
   +-----------------------------------------+-------------------------------------------------------------+--------------+---------------+---------------------------------------------+
