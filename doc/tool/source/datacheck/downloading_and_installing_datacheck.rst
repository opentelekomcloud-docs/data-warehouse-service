:original_name: dws_07_0205.html

.. _dws_07_0205:

Downloading and Installing DataCheck
====================================

Prerequisites
-------------

-  The server is compatible with 64-bit operating systems and can run on either Linux or Windows.

-  Either JDK 1.8 or JRE 1.8 has been installed on the system.
-  The DataCheck tool's server is on the same network as the database to be interconnected with.

Downloading DataCheck
---------------------

Contact technical support team to download the DataCheck software.

Installing DataCheck
--------------------

DataCheck is a command line tool that works on both Linux and Windows OSs. It does not require installation and can be used right after downloading the software package. You can decompress it to start using it.

Windows:

#. Decompress the **DataCheck-*.zip** package.

   The **DataCheck-\*** folder is generated.

   .. note::

      You can decompress **DataCheck-\*** to any folder you need.

#. Go to the **DataCheck-\*** directory.

#. Find and check the files in the DataCheck directory.

   :ref:`Table 1 <en-us_topic_0000001813598956__en-us_topic_0000001290072612_en-us_topic_0218440254_table9595434171818>` describes the obtained folders and files.

**Linux**

#. Extract files from the **DataCheck-*.zip** package.

   .. code-block::

      unzip DataCheck-*.zip

#. Go to the DataCheck directory.

   .. code-block::

      cd DataCheck-*

#. Check the files in the DataCheck directory.

   .. code-block::

      ls
      conf   lib   bin   check_input.xlsx

.. _en-us_topic_0000001813598956__en-us_topic_0000001290072612_en-us_topic_0218440254_table9595434171818:

.. table:: **Table 1** DataCheck directory description

   +-----------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | File or Folder        |                         | Description                                                                                                                                                      |
   +=======================+=========================+==================================================================================================================================================================+
   | DataCheck             | bin                     | Stores the script for starting the tool.                                                                                                                         |
   |                       |                         |                                                                                                                                                                  |
   |                       |                         | Windows version: datacheck.bat                                                                                                                                   |
   |                       |                         |                                                                                                                                                                  |
   |                       |                         | Linux version: datacheck.sh                                                                                                                                      |
   +-----------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | conf                    | Configuration file, which is used to configure the connection between the source database and the destination database and set log printing.                     |
   +-----------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | lib                     | Stores JAR packages required for running the check tool.                                                                                                         |
   +-----------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | check_input.xlsx        | 1. Information about the table to be checked, including the schema name, table name, and column name.                                                            |
   |                       |                         |                                                                                                                                                                  |
   |                       |                         | 2. Check level information and check rules of users. Three verification levels are supported: **high**, **middle**, and **low**. The default value is **low**.   |
   +-----------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | logs                    | The compressed package does not contain this file. After the check tool is executed, this file is automatically generated to record the tool running process.    |
   +-----------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | check_input_result.xlsx | The compressed package does not include this file. Once the check tool is run, a check result file will be created in the same location as **check_input.xlsx**. |
   +-----------------------+-------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------+
