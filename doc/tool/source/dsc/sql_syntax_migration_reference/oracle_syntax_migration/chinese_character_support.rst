:original_name: dws_mt_0222.html

.. _dws_mt_0222:

Chinese Character Support
=========================

**Input-Chinese (**

.. code-block::

   create table test11(a int,b int)/*create table test11(a int,b int)*/;

**Output**

.. code-block::

   CREATE TABLE test11 (a INT,b INT)/*create table test11(a　int,b　int)*/;

**Input-Chinese )**

.. code-block::

   create table test11(a int,b int)/*create table test11(a int,b int)*/;

**Output**

.. code-block::

   CREATE TABLE test11 (a INT,b INT)/*create table test11(a　int,b　int)*/;

**Input-Chinese,**

.. code-block::

   create table test11(a int,b int)/*create table test11(a int,b int)*/;

**Output**

.. code-block::

   CREATE TABLE test11 (a INT,b INT)/*create table test11(a　int,b　int)*/;

**Input-Support Chinese SPACE**

.. code-block::

   create table test11(a int,b int)/*create table test11(a　int,b　int)*/;

**Output**

.. code-block::

   CREATE TABLE test11 (a INT,b INT)/*create table test11(a　int,b　int)*/;
