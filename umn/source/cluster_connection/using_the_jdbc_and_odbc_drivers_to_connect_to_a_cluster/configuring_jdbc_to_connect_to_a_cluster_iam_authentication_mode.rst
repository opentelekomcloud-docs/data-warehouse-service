:original_name: dws_01_0133.html

.. _dws_01_0133:

Configuring JDBC to Connect to a Cluster (IAM Authentication Mode)
==================================================================

Overview
--------

GaussDB(DWS) allows you to access databases using IAM authentication. When you use the JDBC application program to connect to a cluster, set the IAM username, credential, and other information as you configure the JDBC URL. After doing this, when you try to access a database, the system will automatically generate a temporary credential and a connection will be set up.

.. note::

   -  Currently, only clusters 1.3.1 and later versions and their corresponding JDBC drivers can access the databases in IAM authentication mode. Download the JDBC driver. For details, see :ref:`Downloading the JDBC or ODBC Driver <dws_01_0032>`.

IAM supports two types of user credential: password and Access Key ID/Secret Access Key (AK/SK). JDBC connection requires the latter.

The IAM account you use to access a database must be granted with the **DWS Database Access** permission. Only users with both the **DWS Administrator** and **DWS Database Access** permissions can connect to GaussDB(DWS) databases using the temporary database user credentials generated based on IAM users.

The **DWS Database Access** permission can only be granted to user groups. Ensure that your IAM account is in a user group with this permission.

On IAM, only users in the **admin** group have the permissions to manage users. This requires that your IAM account be in the **admin** user group. Otherwise, contact the IAM account administrator to grant your IAM account this permission.

The process of accessing a database is as follows:

#. :ref:`Granting an IAM Account the GaussDB(DWS) Database Access Permission <en-us_topic_0000001707293773__en-us_topic_0000001372839382_section1560842714>`
#. :ref:`Creating an IAM User Credential <en-us_topic_0000001707293773__en-us_topic_0000001372839382_section5410134511612>`
#. :ref:`Configuring the JDBC Connection to Connect to a Cluster Using IAM Authentication <en-us_topic_0000001707293773__en-us_topic_0000001372839382_section289114226329>`

.. _en-us_topic_0000001707293773__en-us_topic_0000001372839382_section1560842714:

Granting an IAM Account the GaussDB(DWS) Database Access Permission
-------------------------------------------------------------------

#. Log in to the cloud management console. In the service list, choose **Management & Governance** > **Identity and Access Management** to enter the IAM management console.

#. Modify the user group to which your IAM user belongs. Set a policy for, grant the **DWS Database Access** permission to, and add your IAM user to it.

   Only users in the **admin** user group of IAM can perform this step. In IAM, only users in the **admin** user group can manage users, including creating user groups and users and setting user group rights.

   For details, see "User and User Group Management > Viewing or Modifying User Group Information" in the *Identity and Access Management User Guide*.

   You can also create an IAM user group, and set a policy for, grant the **DWS Administrator** and **DWS Database Access** permissions to, and add your IAM user to it. For details, see "User and User Group Management > Creating a User Group" in the *Identity and Access Management User Guide*.

.. _en-us_topic_0000001707293773__en-us_topic_0000001372839382_section5410134511612:

Creating an IAM User Credential
-------------------------------

You can log in to the management console to create an AK/SK pair or use an existing one.

#. Log in to the management console.

#. Move the cursor to the username in the upper right corner and choose **My Credentials**.

#. Choose **Access Keys** to view the existing access keys. You can also click **Create Access Key** to create a new one.

   The AK/SK pair is so important that you can download the private key file containing the AK/SK information only when you create the pair. On the management console, you can only view the AKs. If you have not downloaded the file, obtain it from your administrator or create an AK/SK pair again.

   .. note::

      Each user can create a maximum of two AK/SK pairs, which are valid permanently. To ensure account security, change your AK/SK pairs periodically and keep them safe.

.. _en-us_topic_0000001707293773__en-us_topic_0000001372839382_section289114226329:

Configuring the JDBC Connection to Connect to a Cluster Using IAM Authentication
--------------------------------------------------------------------------------

**Configuring JDBC Connection Parameters**

.. table:: **Table 1** Database connection parameters

   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   +===================================+=========================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | url                               | gsjdbc4.jar/gsjdbc200.jar database connection descriptor. The JDBC API does not provide the connection retry capability. You need to implement the retry processing in the service code. The URL example is as follows:                                                                                                                                                                                                                                 |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | .. code-block::                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    jdbc:dws:iam://dws-IAM-demo:eu-de/gaussdb?AccessKeyID=XXXXXXXXXXXXXXXXXXXX&SecretAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX&DbUser=user_test&AutoCreate=true                                                                                                                                                                                                                                                                                     |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | **JDBC URL parameters**:                                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | -  **jdbc:dws:iam** is a prefix in the URL format.                                                                                                                                                                                                                                                                                                                                                                                                      |
   |                                   | -  **dws-IAM-demo** indicates the name of the cluster containing the database.                                                                                                                                                                                                                                                                                                                                                                          |
   |                                   | -  **eu-de** indicates the region where the cluster resides. JDBC accesses the GaussDB(DWS) cluster in the corresponding region and delivers the IAM certificate to the cluster for IAM user authentication. The GaussDB(DWS) service address has been recorded in the JDBC configuration file.                                                                                                                                                         |
   |                                   | -  **gaussdb** indicates the name of the database to which you want to connect.                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | -  **AccessKeyID** and **SecretAccessKey** are the access key ID and secret access key corresponding to the IAM user specified by **DbUser**.                                                                                                                                                                                                                                                                                                           |
   |                                   | -  Set **DbUser** to the IAM username. Note that the current version does not support hyphens (-) in the IAM username.                                                                                                                                                                                                                                                                                                                                  |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    -  If the user specified by **DbUser** exists in the database, the temporary user credential has the same permissions as the existing user.                                                                                                                                                                                                                                                                                                          |
   |                                   |    -  If the user specified by **DbUser** does not exist in the database and the value of **AutoCreate** is **true**, a new user named by the value of **DbUser** is automatically created. The created user is a common database user by default.                                                                                                                                                                                                      |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | -  Parameter **AutoCreate** is optional. The default value is **false**. This parameter indicates whether to automatically create a database user named by the value of **DbUser** in the database.                                                                                                                                                                                                                                                     |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    -  The value **true** indicates that a user is automatically created. If the user already exists, the user will not be created again.                                                                                                                                                                                                                                                                                                                |
   |                                   |    -  The value **false** indicates that a user is not created. If the username specified by **DbUser** does not exist in the database, an error is returned.                                                                                                                                                                                                                                                                                           |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | info                              | Database connection properties. Common properties include the following:                                                                                                                                                                                                                                                                                                                                                                                |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | -  **ssl**: a boolean type. It indicates whether the SSL connection is used.                                                                                                                                                                                                                                                                                                                                                                            |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | -  **loglevel**: an integer type. It sets the log amount recorded in DriverManager for LogStream or LogWriter.                                                                                                                                                                                                                                                                                                                                          |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   |    Currently, **org.postgresql.Driver.DEBUG** and **org.postgresql.Driver.INFO** logs are supported. If the value is **1**, only **org.postgresql.Driver.INFO** (little information) is recorded. If the value is greater than or equal to **2**, **org.postgresql.Driver.DEBUG** and **org.postgresql.Driver.INFO** logs are printed, and detailed log information is generated. Its default value is **0**, which indicates that no logs are printed. |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | -  **charSet**: a string type. It indicates character sets used when data is sent from the database or the database receives data.                                                                                                                                                                                                                                                                                                                      |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                   | -  **prepareThreshold**: an integer type. It is used to determine the execution times of PreparedStatement before the information is converted into prepared statements on the server. The default value is **5**.                                                                                                                                                                                                                                      |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Example**

::

   //The following uses gsjdbc4.jar as an example.
   // The following code encapsulates the database connection obtaining operations into an API. You can connect to the database by specifying the region where the cluster is located, cluster name, access key ID, secret access key, and the corresponding IAM username.
   public static Connection GetConnection(String clustername, String regionname, String AK, String SK,
       String username) {
       // Driver class.
       String driver = "org.postgresql.Driver";
       // Database connection descriptor.
       String sourceURL = "jdbc:dws:iam://" + clustername + ":" + regionname + "/postgresgaussdb?" + "AccessKeyID="
           + AK + "&SecretAccessKey=" + SK + "&DbUser=" + username + "&autoCreate=true";

       Connection conn = null;

       try {
           // Load the driver.
           Class.forName(driver);
       } catch (ClassNotFoundException e) {
           return null;
       }
       try {
           // Create a connection.
           conn = DriverManager.getConnection(sourceURL);
           System.out.println("Connection succeed!");
       } catch (SQLException e) {
           return null;
       }
       return conn;
   }
