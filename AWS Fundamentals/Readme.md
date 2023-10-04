## RDS, Aurora & Elastic search

### Amazon RDS Overview
• RDS stands for Relational Database Service
• It's a managed DB service for DB use SQL as a query language.
• It allows you to create databases in the cloud that are managed by AWS
  - Postgres
  - MySOL
  - MariaDB
  - Oracle
  - Microsoft SOL Server
  - Aurora (AWS Proprietary database)

## Advantage over using RDS versus deploying DB on EC2
• RDS is a managed service:
• Automated provisioning, OS patching
• Continuous backups and restore to specific timestamp (Point in Time Restore)!
• Monitoring dashboards
• Read replicas for improved read performance
• Multi AZ setup for DR (Disaster Recovery)
• Maintenance windows for upgrades
• Scaling capability (vertical and horizontal)
• Storage backed by EBS (gp2 or iol)
• BUT the only cons is we can't SSH into your instances

### RDS - Storage Auto Scaling
• Helps you increase storage on your RDS DB instance
dynamically
• When RDS detects you are running out of free database
storage, it scales automatically
• Avoid manually scaling your database storage
• You have to set Maximum Storage Threshold (maximum limit
for DB storage)
Automatically modify storage if:
• Free storage is less than 10% of allocated storage
• Low-storage lasts at least 5 minutes
• 6 hours have passed since last modification
Advantage over wine RDS versus deploying DR on FC
• Useful for applications with unpredictable
• Supports all RDS database engines (Marial
PostgreSQL, SQL Server, Oracle)

### RDS - Read Replicas vs Multi AZ

### RDS Custom
• Managed Oracle and Microsoft SQL Server Database with OS and
database customization
• RDS: Automates setup, operation, and scaling of database in AWS
• Custom: access to the underlying database and OS so you can
• Configure settings
• Install patches
• Enable native features
• Access the underlying EC2 Instance using SSH or SSM Session Manager
• De-activate Automation Mode to perform your customization,
better to take a DB snapshot before

#### RDS vs. RDS Custom
• RDS: entire database and the OS to be managed by AWS
• RDS Custom: full admin access to the underlying OS and the data

### Amazon Aurora
- Regional Service (supports global databases)
- Supports Multi AZ
- AWS managed Relational DB cluster
- Preferred over Relational Database Service (RDS)
- Auto-scaling (max 128TB)
- Up to 15 read replicas
- Asynchronous Replication (milliseconds)
- Supports only MySQL & PostgreSQL
- Cloud-optimized (5x performance improvement over MySQL on RDS, over 3x the performance of PostgreSQL on RDS)
- Backtrack: restore data at any point of time without taking backups

### Aurora High Avalibility and Scalability
• 6 copies of your data across 3 AZ:
• 4 copies out of 6 needed for writes
• 3 copies out of 6 needed for reads
• Self-healing with peer-to-peer replication
• Storage is striped across 100s of volumes
• One Aurora Instance takes writes (master)
• Automated failover for master in less than 30 seconds
• Master + up to 15 Aurora Read Replicas serve reads
• Support for Cross Region Replication

### Aurora RDS Cluster
#### Endpoints
- Writer Endpoint (Cluster Endpoint)
  - Always points to the master (can be used for read/write)
  - Each Aurora DB cluster has one cluster endpoint
  - If master RDS gets failover then still client will be able to talk with **Writer Endpoint**
- Reader Endpoint
  - Provides load-balancing for read replicas only (used to read only)
  - If the cluster has no read replica, it points to master (can be used to read/write)
  - Each Aurora DB cluster has one reader endpoint
- Custom Endpoint:
  - Used to point to a subset of replicas
  - Provides load-balanced based on criteria other than the read-only or read-write capability of the DB instances like instance class (ex, direct internal users to low-capacity instances and direct production traffic to high-capacity instances)
 
### Features of Aurora
• Automatic fail-over
• Backup and Recovery
• Isolation and security
• Industry compliance
• Push-button scaling
• Automated Patching with Zero Downtime
• Advanced Monitoring
• Routine Maintenance
• Backtrack: restore data at any point of time without using backups

### Aurora Multi-Master
Optional
- Every node (replica) in the cluster can read and write
- Used for immediate failover for write node (high availability in terms of write). If disabled and the master node fails, need t00 promote a Read Replica as the new master (will take some time).
- Client needs to have multiple DB connections for failover

### Aurora Global Database
- Entire database is replicated across regions to recover from region failure
- Designed for globally distributed applications with low latency local reads in each region
- 1 Primary Region (read / write)
- Up to 5 secondary (read-only) regions (replication lag < 1 second)
- Up to 16 Read Replicas per secondary region
- Helps for decreasing latency for clients in other geographical locations
- RTO of less than 1 minute (to promote another region as primary)
