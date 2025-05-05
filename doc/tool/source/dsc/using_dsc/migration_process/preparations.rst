:original_name: dws_16_0019.html

.. _dws_16_0019:

Preparations
============

Before the migration, create an input folder and an output folder, and copy all the SQL scripts to be migrated to the input folder. The following procedure describes how to prepare for the migration in Linux.

#. Run the following commands to create an input folder and an output folder. You can create the folder anywhere based on your preferences. You can also use the default folders for input, output, provided as part of package.

   ::

      mkdir input
      mkdir output

   |image1|

   The tool reads the input folder in batches randomly. After the migration starts, it is recommended the users should not perform any modification on the input folder and files. Abnormal operations will affect the output result of the tool.

#. Copy all SQL scripts to be migrated to the input folder.

   .. note::

      -  If the encoding format of source files is not UTF-8, perform the following steps:

         a. Open the **application.properties** file in the **config** folder.
         b. Change the value of **encodingFormat** in the **application.properties** file to the required encoding format.

         DSC supports the UTF-8, ASCII, and GB2312 encoding formats. The values of **encodingFormat** are case-insensitive.

      -  To obtain the encoding format of a source file in Linux, run the following command on the server where the source file is located:

         ::

            file -bi <Input file name>

.. |image1| image:: /_static/images/danger_3.0-en-us.png
