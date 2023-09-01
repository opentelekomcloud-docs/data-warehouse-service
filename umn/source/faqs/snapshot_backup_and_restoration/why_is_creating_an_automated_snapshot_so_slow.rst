:original_name: dws_03_0059.html

.. _dws_03_0059:

Why Is Creating an Automated Snapshot So Slow?
==============================================

This happens when the data to be backed up is large. Automated snapshots are incremental backups, and the lower the frequency you set (for example, one week), the longer it takes. Increase backup frequency to speed up the process.

The following table lists the snapshot backup and restoration rates. (The rates are obtained from the lab test environment with local SSDs as the backup media. The rates are for reference only. The actual rate depends on your disk, network, and bandwidth resources.)

-  Backup rate: 200 MB/s/DN
-  Restoration rate: 125 MB/s/DN
