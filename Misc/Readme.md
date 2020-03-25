# Miscellaneous Handy Tips

## Virtualbox regenerate HD UUID
```
VBoxManage.exe internalcommands sethduuid 202002-MySQLWorkshop8019-disk002.vdi
```


## Using Samba to exchange files on Windows
Enable samba on linux VM so that you can copy and paste files from your host Windows OS to Linux VM easily
### Install Samba on Linux VM
```
sudo yum install samba
vi /etc/samba/smb.conf

[global]
        workgroup = WORKGROUP
        netbios name = SA
        map to guest = Bad User
[root]
        path = /
        read only = no
        guest ok = yes
        create mask = 0755
        directory mask = 0755
        force user = root
        force group = root
 
 sudo systemctl restart smb
 ```
 ### Map Linux folder on Windows Explorer
 Run "Windows Explorer", Map Network Drive, Specify the IP and the samba folder directive, for example, **192.168.56.41/root**

## NDB Commands
### GET commands
```
get mycluster1;

mcm> get mycluster1;
+-----------------------------+------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
| Name                        | Value                                                | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment   |
+-----------------------------+------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
| Name                        | mycluster1                                           |          |         |          |         |         | Read only |
| DataDir                     | /home/mysql/mcm/mcm_data/clusters/mycluster1/50/data | ndb_mgmd | 50      |          |         |         |           |
| HostName                    | primary                                              | ndb_mgmd | 50      |          |         |         | Read only |
| NodeId                      | 50                                                   | ndb_mgmd | 50      |          |         |         | Read only |
| Arbitration                 | waitexternal                                         | ndbmtd   | 1       |          |         | Process |           |
| DataDir                     | /home/mysql/mcm/mcm_data/clusters/mycluster1/1/data  | ndbmtd   | 1       |          |         |         |           |
| DataMemory                  | 60M                                                  | ndbmtd   | 1       |          |         | Process |           |
| HostName                    | primary                                              | ndbmtd   | 1       |          |         |         | Read only |
| MaxNoOfConcurrentOperations | 100000                                               | ndbmtd   | 1       |          |         | Process |           |
| NodeId                      | 1                                                    | ndbmtd   | 1       |          |         |         | Read only |
| Arbitration                 | waitexternal                                         | ndbmtd   | 2       |          |         | Process |           |
| DataDir                     | /home/mysql/mcm/mcm_data/clusters/mycluster1/2/data  | ndbmtd   | 2       |          |         |         |           |
| DataMemory                  | 60M                                                  | ndbmtd   | 2       |          |         | Process |           |
| HostName                    | primary                                              | ndbmtd   | 2       |          |         |         | Read only |
| MaxNoOfConcurrentOperations | 100000                                               | ndbmtd   | 2       |          |         | Process |           |
| NodeId                      | 2                                                    | ndbmtd   | 2       |          |         |         | Read only |
| datadir                     | /home/mysql/mcm/mcm_data/clusters/mycluster1/49/data | mysqld   | 49      |          |         |         |           |
| HostName                    | primary                                              | mysqld   | 49      |          |         |         | Read only |
| ndb_nodeid                  | 49                                                   | mysqld   | 49      |          |         |         | Read only |
| ndbcluster                  | on                                                   | mysqld   | 49      |          |         |         | Read only |
| NodeId                      | 49                                                   | mysqld   | 49      |          |         |         | Read only |
| port                        | 3316                                                 | mysqld   | 49      |          |         | Process |           |
| socket                      | /tmp/mysql.mycluster1.49.sock                        | mysqld   | 49      |          |         |         |           |
| tmpdir                      | /home/mysql/mcm/mcm_data/clusters/mycluster1/49/tmp  | mysqld   | 49      |          |         |         |           |
| NodeId                      | 51                                                   | ndbapi   | 51      |          |         |         | Read only |
| NodeId                      | 52                                                   | ndbapi   | 52      |          |         |         | Read only |
| NodeId                      | 53                                                   | ndbapi   | 53      |          |         |         | Read only |
+-----------------------------+------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
27 rows in set (0.02 sec)


get hostname mycluster1;
mcm> get hostname mycluster1;
+----------+---------+----------+---------+----------+---------+-------+-----------+
| Name     | Value   | Process1 | NodeId1 | Process2 | NodeId2 | Level | Comment   |
+----------+---------+----------+---------+----------+---------+-------+-----------+
| HostName | primary | ndb_mgmd | 50      |          |         |       | Read only |
| HostName | primary | ndbmtd   | 1       |          |         |       | Read only |
| HostName | primary | ndbmtd   | 2       |          |         |       | Read only |
| HostName | primary | mysqld   | 49      |          |         |       | Read only |
+----------+---------+----------+---------+----------+---------+-------+-----------+
4 rows in set (0.11 sec)


get Hostname:ndbmtd mycluster1;
mcm> get Hostname:ndbmtd mycluster1;
+----------+---------+----------+---------+----------+---------+-------+-----------+
| Name     | Value   | Process1 | NodeId1 | Process2 | NodeId2 | Level | Comment   |
+----------+---------+----------+---------+----------+---------+-------+-----------+
| HostName | primary | ndbmtd   | 1       |          |         |       | Read only |
| HostName | primary | ndbmtd   | 2       |          |         |       | Read only |
+----------+---------+----------+---------+----------+---------+-------+-----------+
2 rows in set (0.11 sec)

get H* mycluster1;
mcm> get H* mycluster1;
+----------+---------+----------+---------+----------+---------+-------+-----------+
| Name     | Value   | Process1 | NodeId1 | Process2 | NodeId2 | Level | Comment   |
+----------+---------+----------+---------+----------+---------+-------+-----------+
| HostName | primary | ndb_mgmd | 50      |          |         |       | Read only |
| HostName | primary | ndbmtd   | 1       |          |         |       | Read only |
| HostName | primary | ndbmtd   | 2       |          |         |       | Read only |
| HostName | primary | mysqld   | 49      |          |         |       | Read only |
+----------+---------+----------+---------+----------+---------+-------+-----------+
4 rows in set (0.04 sec)


get DataMemory:ndbmtd mycluster1;
mcm> get DataMemory:ndbmtd mycluster1;
+------------+-------+----------+---------+----------+---------+---------+---------+
| Name       | Value | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment |
+------------+-------+----------+---------+----------+---------+---------+---------+
| DataMemory | 60M   | ndbmtd   | 1       |          |         | Process |         |
| DataMemory | 60M   | ndbmtd   | 2       |          |         | Process |         |
+------------+-------+----------+---------+----------+---------+---------+---------+
2 rows in set (0.06 sec)


get *:mysqld mycluster1;
mcm> get *:mysqld mycluster1;
+------------+------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
| Name       | Value                                                | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment   |
+------------+------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
| datadir    | /home/mysql/mcm/mcm_data/clusters/mycluster1/49/data | mysqld   | 49      |          |         |         |           |
| HostName   | primary                                              | mysqld   | 49      |          |         |         | Read only |
| ndb_nodeid | 49                                                   | mysqld   | 49      |          |         |         | Read only |
| ndbcluster | on                                                   | mysqld   | 49      |          |         |         | Read only |
| NodeId     | 49                                                   | mysqld   | 49      |          |         |         | Read only |
| port       | 3316                                                 | mysqld   | 49      |          |         | Process |           |
| socket     | /tmp/mysql.mycluster1.49.sock                        | mysqld   | 49      |          |         |         |           |
| tmpdir     | /home/mysql/mcm/mcm_data/clusters/mycluster1/49/tmp  | mysqld   | 49      |          |         |         |           |
+------------+------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
8 rows in set (0.05 sec)

get -d ndb*:mysqld mycluster1;
mcm> get -d ndb*:mysqld mycluster1;
+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
| Name                                 | Value                                                                                                                                                                                                                                                            | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment   |
+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
| ndb_allow_copying_alter_table        | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_autoincrement_prefetch_sz        | 1                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_batch_size                       | 32768                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_blob_read_batch_bytes            | 65536                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_blob_write_batch_bytes           | 65536                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_cache_check_time                 | 0                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_cluster_connection_pool          | 1                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_cluster_connection_pool_nodeids  | NULL                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_connectstring                    |                                                                                                                                                                                                                                                                  | mysqld   | 49      |          |         | Default |           |
| ndb_data_node_neighbour              | 0                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_dbg_check_shares                 | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_default_column_format            | FIXED                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_deferred_constraints             | 0                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_distribution                     | keyhash                                                                                                                                                                                                                                                          | mysqld   | 49      |          |         | Default |           |
| ndb_eventbuffer_free_percent         | 20                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndb_eventbuffer_max_alloc            | 0                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_extra_logging                    | 1                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_force_send                       | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_fully_replicated                 | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_index_stat_enable                | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_index_stat_option                | loop_enable=1000ms,loop_idle=1000ms,loop_busy=100ms,update_batch=1,read_batch=4,idle_batch=32,check_batch=8,check_delay=10m,delete_batch=8,clean_delay=1m,error_batch=4,error_delay=1m,evict_batch=8,evict_delay=1m,cache_limit=32M,cache_lowpct=90,zero_total=0 | mysqld   | 49      |          |         | Default |           |
| ndb_join_pushdown                    | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_log_apply_status                 | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_log_bin                          | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_log_binlog_index                 | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_log_empty_epochs                 | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_log_empty_update                 | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_log_exclusive_reads              | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_log_orig                         | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_log_transaction_id               | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_log_update_as_write              | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_log_update_minimal               | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_log_updated_only                 | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_mgmd_host                        | localhost:1186                                                                                                                                                                                                                                                   | mysqld   | 49      |          |         | Default |           |
| ndb_nodeid                           | 49                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         |         | Read only |
| ndb_optimization_delay               | 10                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndb_optimized_node_selection         | 3                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_read_backup                      | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_recv_thread_activation_threshold | 8                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_recv_thread_cpu_mask             |                                                                                                                                                                                                                                                                  | mysqld   | 49      |          |         | Default |           |
| ndb_report_thresh_binlog_epoch_slip  | 10                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndb_report_thresh_binlog_mem_usage   | 10                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndb_row_checksum                     | 1                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndb_show_foreign_key_mock_tables     | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_slave_conflict_role              | none                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_table_no_logging                 | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_table_temporary                  | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_transid_mysql_connection_map     | on                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndb_use_copying_alter_table          | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_use_exact_count                  | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
| ndb_use_transactions                 | true                                                                                                                                                                                                                                                             | mysqld   | 49      |          |         | Default |           |
| ndb_wait_connected                   | 30                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndb_wait_setup                       | 30                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndbcluster                           | on                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         |         | Read only |
| ndbinfo                              | on                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndbinfo_max_bytes                    | 0                                                                                                                                                                                                                                                                | mysqld   | 49      |          |         | Default |           |
| ndbinfo_max_rows                     | 10                                                                                                                                                                                                                                                               | mysqld   | 49      |          |         | Default |           |
| ndbinfo_show_hidden                  | false                                                                                                                                                                                                                                                            | mysqld   | 49      |          |         | Default |           |
+--------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+---------+----------+---------+---------+-----------+
58 rows in set (0.08 sec)


get -d *memory*:ndbmtd:1 mycluster1;
mcm> get -d *memory*:ndbmtd:1 mycluster1;
+-------------------------+-----------+----------+---------+----------+---------+---------+---------+
| Name                    | Value     | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment |
+-------------------------+-----------+----------+---------+----------+---------+---------+---------+
| BackupMemory            | 33554432  | ndbmtd   | 1       |          |         | Default |         |
| DataMemory              | 60M       | ndbmtd   | 1       |          |         | Process |         |
| DiskPageBufferMemory    | 67108864  | ndbmtd   | 1       |          |         | Default |         |
| ExtraSendBufferMemory   | 0         | ndbmtd   | 1       |          |         | Default |         |
| IndexMemory             | 0         | ndbmtd   | 1       |          |         | Default |         |
| LockPagesInMainMemory   | 0         | ndbmtd   | 1       |          |         | Default |         |
| SharedGlobalMemory      | 134217728 | ndbmtd   | 1       |          |         | Default |         |
| StringMemory            | 25        | ndbmtd   | 1       |          |         | Default |         |
| TotalSendBufferMemory   | 0         | ndbmtd   | 1       |          |         | Default |         |
| TransactionBufferMemory | 1048576   | ndbmtd   | 1       |          |         | Default |         |
+-------------------------+-----------+----------+---------+----------+---------+---------+---------+
10 rows in set (0.03 sec)


get -d port:mysqld mycluster1;
+------+-------+----------+---------+----------+---------+---------+---------+
| Name | Value | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment |
+------+-------+----------+---------+----------+---------+---------+---------+
| port | 3316  | mysqld   | 49      |          |         | Process |         |
+------+-------+----------+---------+----------+---------+---------+---------+
1 row in set (0.05 sec)

get -d default_storage_engine mycluster1;
+------------------------+--------+----------+---------+----------+---------+---------+---------+
| Name                   | Value  | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment |
+------------------------+--------+----------+---------+----------+---------+---------+---------+
| default_storage_engine | innodb | mysqld   | 49      |          |         | Default |         |
+------------------------+--------+----------+---------+----------+---------+---------+---------+
1 row in set (0.06 sec)


get --include-defaults DataMemory:ndbmtd mycluster1;
mcm> get --include-defaults DataMemory:ndbmtd mycluster1;
+------------+-------+----------+---------+----------+---------+---------+---------+
| Name       | Value | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment |
+------------+-------+----------+---------+----------+---------+---------+---------+
| DataMemory | 60M   | ndbmtd   | 1       |          |         | Process |         |
| DataMemory | 60M   | ndbmtd   | 2       |          |         | Process |         |
+------------+-------+----------+---------+----------+---------+---------+---------+
2 rows in set (0.08 sec)

get replicate_ignore_table:mysqld mycluster1;
mcm> get replicate_ignore_table:mysqld mycluster1;
Empty set (0.11 sec)

get -d wait_timeout:mysqld mycluster1;
mcm> get -d wait_timeout:mysqld mycluster1;
+--------------+-------+----------+---------+----------+---------+---------+---------+
| Name         | Value | Process1 | NodeId1 | Process2 | NodeId2 | Level   | Comment |
+--------------+-------+----------+---------+----------+---------+---------+---------+
| wait_timeout | 28800 | mysqld   | 49      |          |         | Default |         |
+--------------+-------+----------+---------+----------+---------+---------+---------+
1 row in set (0.03 sec)

```
### List commands
```
list sites;
mcm> list sites;
+--------+------+-------+---------+
| Site   | Port | Local | Hosts   |
+--------+------+-------+---------+
| mysite | 1862 | Local | primary |
+--------+------+-------+---------+
1 row in set (0.03 sec)


list clusters mysite;

mcm> list clusters mysite;
+------------+-------------+
| Cluster    | Package     |
+------------+-------------+
| mycluster1 | cluster.pkg |
+------------+-------------+
1 row in set (0.02 sec)


list nextnodeids mycluster1;
mcm> list nextnodeids mycluster1;
+-----------+--------------+-------------+--------------------------+
| Category  | NodeId Range | Next NodeId | Processes                |
+-----------+--------------+-------------+--------------------------+
| Datanodes | 1  - 48      | 3           | ndbd, ndbmtd             |
| Others    | 49 - 255     | 54          | ndb_mgmd, mysqld, ndbapi |
+-----------+--------------+-------------+--------------------------+
2 rows in set (0.07 sec)

list processes mycluster1;
mcm> list processes mycluster1;
+--------+----------+---------+
| NodeId | Name     | Host    |
+--------+----------+---------+
| 50     | ndb_mgmd | primary |
| 1      | ndbmtd   | primary |
| 2      | ndbmtd   | primary |
| 49     | mysqld   | primary |
| 51     | ndbapi   | *       |
| 52     | ndbapi   | *       |
| 53     | ndbapi   | *       |
+--------+----------+---------+
7 rows in set (0.02 sec)


mcm> list processes mycluster1;
+--------+----------+---------+
| NodeId | Name     | Host    |
+--------+----------+---------+
| 50     | ndb_mgmd | primary |
| 1      | ndbmtd   | primary |
| 2      | ndbmtd   | primary |
| 49     | mysqld   | primary |
| 51     | ndbapi   | *       |
| 52     | ndbapi   | *       |
| 53     | ndbapi   | *       |
+--------+----------+---------+
7 rows in set (0.04 sec)

mcm> list packages mysite;
+-------------+--------------------------------+---------+
| Package     | Path                           | Hosts   |
+-------------+--------------------------------+---------+
| cluster.pkg | /home/mysql/mcm/cluster-latest | primary |
+-------------+--------------------------------+---------+
1 row in set (0.09 sec)


list backups mycluster1;
mcm> list backups mycluster1;
+----------+--------+---------+-----------+-------+---------------------+
| BackupId | NodeId | Host    | Timestamp | Parts | Comment             |
+----------+--------+---------+-----------+-------+---------------------+
| None     | 1      | primary |           |       | No backup directory |
| None     | 2      | primary |           |       | No backup directory |
+----------+--------+---------+-----------+-------+---------------------+
2 rows in set (0.01 sec)
```
### Show Status commands
```
show status -r mycluster1;
mcm> show status -r mycluster1;
+--------+----------+---------+---------+-----------+-------------+
| NodeId | Process  | Host    | Status  | Nodegroup | Package     |
+--------+----------+---------+---------+-----------+-------------+
| 50     | ndb_mgmd | primary | running |           | cluster.pkg |
| 1      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 2      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 49     | mysqld   | primary | running |           | cluster.pkg |
| 51     | ndbapi   | *       | added   |           |             |
| 52     | ndbapi   | *       | added   |           |             |
| 53     | ndbapi   | *       | added   |           |             |
+--------+----------+---------+---------+-----------+-------------+
7 rows in set (0.05 sec)


show status -b mycluster1;

mcm> show status -b mycluster1;
+---------------+----------+-----------------------------+
| Command       | Status   | Progress                    |
+---------------+----------+-----------------------------+
| start cluster | finished | 100% [####################] |
+---------------+----------+-----------------------------+
1 row in set (0.02 sec)

show status --cluster mycluster1;

mcm> show status --cluster mycluster1;
+------------+-------------------+---------+
| Cluster    | Status            | Comment |
+------------+-------------------+---------+
| mycluster1 | fully operational |         |
+------------+-------------------+---------+
1 row in set (0.06 sec)

show status --operation mycluster1;
mcm> show status --operation mycluster1;
+---------------+----------+--------------+
| Command       | Status   | Description  |
+---------------+----------+--------------+
| start cluster | finished | <no message> |
+---------------+----------+--------------+
1 row in set (0.05 sec)


show status --backup mycluster1;

mcm> show status --backup mycluster1;
+------------------------------------------+
| Command result                           |
+------------------------------------------+
| No backup currently active in mycluster1 |
+------------------------------------------+
1 row in set (0.07 sec)

show status --process mycluster1;

mcm> show status --progress mycluster1;
+---------------+----------+----------+
| Command       | Status   | Progress |
+---------------+----------+----------+
| start cluster | finished | 100%     |
+---------------+----------+----------+
1 row in set (0.03 sec)


mcm> show status --process mycluster1;
+--------+----------+---------+---------+-----------+-------------+
| NodeId | Process  | Host    | Status  | Nodegroup | Package     |
+--------+----------+---------+---------+-----------+-------------+
| 50     | ndb_mgmd | primary | running |           | cluster.pkg |
| 1      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 2      | ndbmtd   | primary | running | 0         | cluster.pkg |
| 49     | mysqld   | primary | running |           | cluster.pkg |
| 51     | ndbapi   | *       | added   |           |             |
| 52     | ndbapi   | *       | added   |           |             |
| 53     | ndbapi   | *       | added   |           |             |
+--------+----------+---------+---------+-----------+-------------+
7 rows in set (0.08 sec)

show status --progress mycluster1;
mcm> show status --progress mycluster1;
+---------------+----------+----------+
| Command       | Status   | Progress |
+---------------+----------+----------+
| start cluster | finished | 100%     |
+---------------+----------+----------+
1 row in set (0.04 sec)
```

### Start/Stop Process
```
stop process 51 mycluster1; 
start process 51 mycluster1; 
```

### Rolling Upgrade
```
mcm
add package --basedir=/opt/mcm/cluster7.6.11 7.6.11;
upgrade cluster --package=7.6.11 mycluster1;
delete package 7.6.8;
list packages mysite;
quit
```

### Add new hosts
```
mcm
add hosts --hosts=xxxx mysite;
add package --hosts=xxxx --basedir=/opt/mcm/cluster7.6.11 7.6.11;
add process -R ndbmtd@x.x.x.x,ndbmtd@y.y.y.y,mysqld@z.z.z.z mycluster1;
start process --added mycluster1;
quit
```
### mysqlslap
```
/opt/mcm/cluster/bin/mysqlslap --protocol tcp -h 10.0.10.10 --concurrency=50 --auto-generate-sql-execute-number 10000 \
--number-int-cols=2 --number-char-cols=3 --auto-generate-sql --engine=ndb --no-drop --auto-generate-sql-add-autoincrement \
--auto-generate-sql-load-type=write --auto-generate-sql-secondary-indexes=2 --auto-generate-sql-unique-write-number=500
```



