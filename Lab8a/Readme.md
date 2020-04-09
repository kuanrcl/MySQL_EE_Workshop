# High Availability with Local- and Cross-site replication
We will configure local replication and cross-site replication to mimic real-life deployment of async replication in production data center and cross-site replication from production data center to disaster recovery data center. In this scenario, Prod DC will have A->B and DRC will have C->D, and A->C
## Local Replicatin (MySQL Server A and B)
To simulate multiple MySQL servers in a single VM, we will need to specify different **server_id** and **port**
