:original_name: dws_04_0441.html

.. _dws_04_0441:

Selecting a Distribution Column
===============================

The distribution column in a hash table must meet the following requirements, which are ranked by priority in descending order:

#. **The value of the distribution column should be discrete so that data can be evenly distributed on each DN.** For example, you are advised to select the primary key of a table as the distribution column, and the ID card number as the distribution column in a personnel information table.

#. **Do not select the column where a constant filter exists.** For example, if a constant constraint (for example, zqdh= '000001') exists on the **zqdh** column in some queries on the **dwcjk** table, you are not advised to use **zqdh** as the distribution column.

#. **Select the join condition as the distribution column**, so that join tasks can be pushed down to DNs to execute, reducing the amount of data transferred between the DNs.

   For a hash table, an improper distribution key may cause data skew or poor I/O performance on certain DNs. Therefore, you need to check the table to ensure that data is evenly distributed on each DN. You can run the following SQL statements to check data skew:

   ::

      select
      xc_node_id, count(1)
      from tablename
      group by xc_node_id
      order by xc_node_id desc;

   **xc_node_id** corresponds to a DN. Generally, **over 5% difference between the amount of data on different DNs is regarded as data skew. If the difference is over 10%, choose another distribution column.**

#. You are not advised to add a column as a distribution column, especially add a new column and use the SEQUENCE value to fill the column. This is because SEQUENCE may cause performance bottlenecks and unnecessary maintenance costs.

Multiple distribution columns can be selected in GaussDB(DWS) to evenly distribute data.
