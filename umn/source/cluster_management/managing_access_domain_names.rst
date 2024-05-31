:original_name: dws_01_0140.html

.. _dws_01_0140:

Managing Access Domain Names
============================

Overview
--------

A domain name is a string of characters separated by dots to identify the location of a computer or a computer group on the Internet, for example, www.example.com. You can enter a domain name in the address box of the web browser to access a website or web application.

On GaussDB(DWS), you can access clusters using the private network domain name or the public network domain name.

Private network domain name: Name of the domain for accessing the database in the cluster through the private network. The private network domain name is automatically generated when you create a cluster. The default naming rule is *cluster name.*\ dws.otc-tsi.de. If the cluster name does not comply with the domain name standards, the prefix of the default access domain name will be adjusted accordingly.

Public network domain name: Name of the domain for accessing the database in the cluster through the public network. If a cluster is not bound to an EIP, it cannot be accessed using the public network domain name. If you bind an EIP during cluster creation, the public network domain name is automatically generated. The default naming rule is *cluster name*.dws.t-systems.com.

.. note::

   Neither public nor private domain names support load balancing. To use load balancing, see Configuring JDBC to Connect to a Cluster (Load Balancing Mode).

After a cluster is created, you can set private and public domain names for accessing the cluster as required. The operations are as follows:

-  :ref:`Modifying a Private Network Domain Name <en-us_topic_0000001707254665__en-us_topic_0000001422959333_section1443581220337>`
-  :ref:`Creating a Public Network Domain Name <en-us_topic_0000001707254665__en-us_topic_0000001422959333_section14447182917335>`
-  :ref:`Modifying a Public Network Domain Name <en-us_topic_0000001707254665__en-us_topic_0000001422959333_section220113419330>`
-  :ref:`Releasing a Public Network Domain Name <en-us_topic_0000001707254665__en-us_topic_0000001422959333_section1267743817334>`

.. _en-us_topic_0000001707254665__en-us_topic_0000001422959333_section1443581220337:

Modifying a Private Network Domain Name
---------------------------------------

The private network domain name is automatically generated during cluster creation. After the cluster is created, you can modify the default domain name based on site requirements.

To modify the private network domain name, perform the following steps:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.

#. In the **Connection** area, click **Modify** next to the automatically generated **Private Network Domain Name**.

#. In the **Modify Private Network Domain Name** dialog box, enter the target domain name and click **OK**.

   The private network domain name contains 4 to 63 characters, which consists of letters, digits, and hyphens (-) and must start with a letter.

   After the domain name is modified, click copy button |image1| next to the private network domain name to copy it.

.. _en-us_topic_0000001707254665__en-us_topic_0000001422959333_section14447182917335:

Creating a Public Network Domain Name
-------------------------------------

A cluster is not bound to an EIP by default during cluster creation. That is, cluster access using the public network is disabled. After a cluster is created, if you want to access it over the public network, bind an EIP to the cluster and create a public network domain name.

.. note::

   By default, only cloud accounts or users with Security Administrator permissions can query and create agencies. By default, the IAM users in those accounts cannot query or create agencies. When the users use the EIP, the system makes the binding function unavailable. Contact a user with the **DWS Administrator** permissions to authorize the agency on the current page.

To create a public network domain name, perform the following steps:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.

#. In the **Connection** area, **Public Network Domain Name** and **Public Network IP Address** are empty. Click **Edit** to bind the cluster with an EIP.

#. In the **Edit Elastic IP** dialog box, select an EIP from the drop-down list to bind it to a specified CN.

   If no available EIPs are displayed, click **View EIP** to go to the **Elastic IP** page and create an EIP that satisfies your needs. After the new EIP is created, click the refresh icon next to the drop-down list. The newly created EIP will be displayed in the **EIP** drop-down list.

   After the EIP is bound successfully, the specific public network IP address is displayed in the **Connection** area.

   |image2|

#. In the **Connection** area, click **Create** next to **Public Network Domain Name** to create a public network domain name for the cluster.

#. In the **Apply for Public Network Domain Name** dialog box, enter the target domain name and click **OK**.

   The public network domain name contains 4 to 63 characters, which consists of letters, digits, and hyphens (-) and must start with a letter.

   The specific public network domain name is displayed in the **Connection** area after being created. Click copy button |image3| to copy the public network domain name.

.. _en-us_topic_0000001707254665__en-us_topic_0000001422959333_section220113419330:

Modifying a Public Network Domain Name
--------------------------------------

If you bind an EIP during cluster creation, the public network domain name is automatically generated. After a cluster is created, you can modify the public network domain name as required.

To modify the public network domain name, perform the following steps:

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.
#. Click **Modify** next to the **Public Network Domain Name** in the **Connection** area.
#. In the **Modify Public Network Domain Name** dialog box, enter the target domain name and click **OK**.

.. _en-us_topic_0000001707254665__en-us_topic_0000001422959333_section1267743817334:

Releasing a Public Network Domain Name
--------------------------------------

After a cluster is created, you can release unnecessary public network domain names.

To do so, perform the following steps:

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.
#. Click **Release** next to the **Public Network Domain Name** in the **Connection** area.
#. In the **Release Domain Name** dialog box, click **Yes**.

.. |image1| image:: /_static/images/en-us_image_0000001711440216.png
.. |image2| image:: /_static/images/en-us_image_0000001711599708.png
.. |image3| image:: /_static/images/en-us_image_0000001711440216.png
