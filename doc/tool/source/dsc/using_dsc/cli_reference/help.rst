:original_name: dws_16_0031.html

.. _dws_16_0031:

HELP
====

Functionality
-------------

The **help** command is used to provide the help information for the commands supported by DSC.

Syntax
------

**Linux**:

.. code-block::

   ./runDSC.sh --help

**Windows**:

.. code-block::

   runDSC.bat --help

Examples
--------

**Linux**:

.. code-block::

   ./runDSC.sh --help

**Windows**:

.. code-block::

   runDSC.bat --help

System Response
---------------

**Linux**:

.. code-block::

   ./runDSC.sh --help
   To migrate teradata/oracle/netezza/mysql/db2 database scripts to DWS
   runDSC.sh -S <source-database> [-T <target-database>] -I <input-script-path> -O <output-script-path> [-M <conversion-type>]  [-A <application-lang>]  [-L <log-path>]  [-VN <version-number>]


       -S | --source-db
       The source database, which can be either Teradata or Oracle or Netezza or MySql or DB2.


       -T | --target-db
       The target database, which can be either GaussDBT or GaussDBA.


       -I | --input-folder
       The input/source folder that contains the Teradata/Oracle/Netezza/MySql/DB2 scripts to be migrated.


       -O | --output-folder
       The output/target folder where the migrated scripts are placed.


       -M | --conversion-type
       The conversion type, which can be either Bulk or BLogic.


       -A | --application-lang
       The application language type, which can be either SQL or Perl.


       -L | --log-folder
       The log file path where the log files are created.


       -VN | --version-number
       The version number, which can be either V1R7 or V1R8_330.

      " -P | --pre-execution-path%n" + "
       Path containig the dependent Objects scripts which needs to be executed before the verification step.%n" + "%n" +



   To display DSC version details
   runDSC.sh -V | --version


   To display DSC help details
   runDSC.sh -H | --help


   [Internal] To guess encoding for a file
   runDSC.sh guessencoding -F <filename>


       -F | --file-name
       The filename for which the encoding must be guessed.


   Refer the user manual for more details.

**Windows**:

.. code-block::

   runDSC.bat --help
   To migrate teradata/oracle/netezza/mysql/db2 database scripts to DWS
   runDSC.bat -S <source-database> [-T <target-database>] -I <input-script-path> -O <output-script-path> [-M <conversion-type>]  [-A <application-lang>]  [-L <log-path>]  [-VN <version-number>]


       -S | --source-db
       The source database, which can be either Teradata or Oracle or Netezza or MySql or DB2.


       -T | --target-db
       The target database, which can be either GaussDBT or GaussDBA.


       -I | --input-folder
       The input/source folder that contains the Teradata/Oracle/Netezza/MySql/DB2 scripts to be migrated.


       -O | --output-folder
       The output/target folder where the migrated scripts are placed.


       -M | --conversion-type
       The conversion type, which can be either Bulk or BLogic.


       -A | --application-lang
       The application language type, which can be either SQL or Perl.


       -L | --log-folder
       The log file path where the log files are created.


       -VN | --version-number
       The version number, which can be either V1R7 or V1R8_330.


   To display DSC version details
   runDSC.sh -V | --version


   To display DSC help details
   runDSC.sh -H | --help


   [Internal] To guess encoding for a file
   runDSC.sh guessencoding -F <filename>


       -F | --file-name
       The filename for which the encoding must be guessed.


   Refer the user manual for more details.
