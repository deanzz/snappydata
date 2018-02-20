# Upgrade Instructions (1.0.0 to 1.0.1)

This guide provides information for upgrading systems running an earlier version of SnappyData. We assume that you have SnappyData already installed, and you are upgrading to the latest version of SnappyData.

Before you begin the upgrade, ensure that you understand the new features and any specific requirements for that release.

## Before you begin the upgrade

1. Confirm that your system meets the hardware and software requirements described in [System Requirements](../install/system_requirements.md) section.

2. Backup the existing environment: </br>
Create a backup of the locator, lead, and server configuration files that exist in the **conf** folder located in the SnappyData home directory.

3. Stop the cluster and verify that all members are stopped: You can shut down the cluster using the `sbin/snappy-stop-all.sh` command. </br>To ensure that all the members have been shut down correctly, use the `sbin/snappy-status-all.sh` command.

4. Create a [backup of the operational disk store files](../reference/command_line_utilities/store-backup.md) for all members in the distributed system.

5. Reinstall SnappyData: After you have stopped the cluster, [install the latest version of SnappyData](../install.md).

6. Reconfigure your cluster using the locator, lead, and server configuration files you backed up in step 1.

7. To ensure that the restore script (restore.sh restore.bat) copies files back to their original locations, make sure that the disk files are available at the original location before restarting the cluster with the latest version of SnappyData.


## Additional Steps

For best performance, it is recommended that you re-create any large column tables after you upgrade from 1.0.0 to 1.0.1. The following two improvements provided in 1.0.1 take effect:

* Compression for on-disk and over the network data.

* Separate disk-store for column delta store. This improves the compactor performance significantly for cases of frequent JDBC/ODBC inserts or small inserts where the delta store gets used frequently.

!!! Note:
	- Ensure that no operations are currently running on the system.

	- Use a path for parquet that has enough space to hold the table data, which needs to be deleted explicitly once re-import is successfully done.

The following example demonstrates how you can store/load from parquet:

```
snappy> create external table table1Parquet using parquet options (path '...') as select * from table1;
```
```
snappy> drop table table1;
```

```
snappy> create table table1 ...;
```

```
snappy> insert into table1 as select * from table1Parquet;
```

```
snappy> drop table table1Parquet;
```
