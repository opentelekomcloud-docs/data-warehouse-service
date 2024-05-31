:original_name: dws_04_0442.html

.. _dws_04_0442:

Using Partial Clustering
========================

Partial Cluster Key is the column-based technology. It can minimize or maximize sparse indexes to quickly filter base tables. Partial cluster key can specify multiple columns, but you are advised to specify no more than two columns. Use the following principles to specify columns:

#. The selected columns must be restricted by simple expressions in base tables. Such constraints are usually represented by Col, Op, and Const. Col specifies the column name, Op specifies operators, (including =, >, >=, <=, and <) Const specifies constants.
#. Select columns that are frequently selected (to filter much more undesired data) in simple expressions.
#. List the less frequently selected columns on the top.
#. List the columns of the enumerated type at the top.
