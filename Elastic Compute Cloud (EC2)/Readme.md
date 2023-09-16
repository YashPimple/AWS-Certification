- Regional Service
- EC2 (Elastic Compute Cloud) is an Infrastructure as a Service (IaaS)
- Stopping & Starting an instance may change its public IP but not its private IP
- AWS Compute Optimizer recommends optimal AWS Compute resources for your workloads
- There is a vCPU-based On-Demand Instance soft limit per region

## EC2 Instances Purchasing Options

• On-Demand Instances - short workload, predictable pricing, pay by second
• Reserved (I & 3 years)
• Reserved Instances - long workloads
• Convertible Reserved Instances - long workloads with flexible instances
• Savings Plans (I & 3 years) -commitment to an amount of usage, long workload
• Spot Instances - short workloads, cheap, can lose instances (less reliable)
• Dedicated Hosts - book an entire physical server, control instance placement
• Dedicated Instances - no other customers will share your hardware
• Capacity Reservations - reserve capacity in a specific AZ for any duration

## EC2 Spot Instance Requests

• Can get a discount of up to 90% compared to On-demand
• Define max spot price and get the instance while the current spot price < max
• The hourly spot price varies based on offer and capacity
• If the current spot price > your max price you can choose to stop or terminate you
instance with a 2 minutes grace period.
• Other strategy: Spot Block
• "block" spot instance during a specified time frame (I to 6 hours) without interruptions
• In rare situations, the instance may be reclaimed
• Used for batch jobs, data analysis, or workloads that are resilient to failures.
• Not great for critical jobs or databases

## Spot Fleets

• Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
• The Spot Fleet will try to meet the target capacity with price constraints
• Define possible launch pools: instance type (m5.large), OS, Availability Zone
• Can have multiple launch pools, so that the fleet can choose
• Spot Fleet stops launching instances when reaching capacity or max cost
• Strategies to allocate Spot Instances:
• lowest price: from the pool with the lowest price (cost optimization, short workload)
• Diversified: distributed across all pools (great for availability, long workloads)
• capacity-optimized: pool with the optimal capacity for the number of instances
• priceCapacityOptimized (recommended): pools with the highest capacity available, then select
the pool with the lowest price (best choice for most workloads)
• Spot Fleets allow us to automatically request Spot Instances with the lowest price Fu

## Public and Private IPs

## Elastic IP
- Static Public IP that you own as long as you don't delete it
- Can be attached to an EC2 instance (even when it is stopped)
- Soft limit of 5 elastic IPs per account
- Doesn’t incur charges as long as the following conditions are met (EIP behaving like any other public IP randomly assigned to an EC2 instance):
  - The Elastic IP is associated with an Amazon EC2 instance
  - The instance associated with the Elastic IP is running
  - The instance has only one Elastic IP attached to it
- It is not used mainly since it reflects poor architectural decisions. Instead, use a random public IP and register a DNS name to it.
- Or else use a Loadbalancer and don't use a Public IP

## Elastic Network Interface (ENI)
- **ENI** is a virtual network card that gives a **private IP** to an EC2 instance.
- A primary ENI is created and attached to the instance upon creation and will be deleted automatically upon instance termination.
- Additionally, We can create additional ENIs and attach them to an EC2 instance to access it via multiple private IPs.
- Also We can detach & attach ENIs across instances
- ENIs are tied to the subnet (and hence to the AZ)

## Instance States
1. **Stop**
  - EBS root volume is preserved
  - Instance is temporarily stopped
2. **Terminate**
  - EBS root volume gets destroyed along with the Instance
3. **Hibernate**
  - Hibernation saves the contents from the instance memory (RAM) to the EBS root volume
  - EBS root volume is preserved and needs to be encrypted
  - The instance boots much faster as the OS is not stopped and restarted
  - When you start your instance:
      - EBS root volume is restored to its previous state
      - RAM contents are reloaded
      - Processes that were previously running on the instance are resumed
      - Previously attached data volumes are reattached and the instance retains its instance ID
   - Should be used for applications that take a long time to start
   - Not supported for Spot Instances
   - Max hibernation duration = 60 days

## EC2 Instance Store:
- Hardware storage directly attached to EC2 instance (cannot be detached and attached to another instance)
- Highest IOPS of any available storage (millions of IOPS)
- Ephemeral storage (loses data when the instance is stopped, hibernated or terminated)
- Good for buffer / cache / scratch data / temporary content
- AMI created from an instance does not have its instance store volume preserved
> You can specify the instance store volumes only when you launch an instance. You can’t attach instance store volumes to an instance after you’ve launched it.

## EBS Volume Types
 EBS Volumes come in 6 types
1. gp2. / gp3 (SSD): General purpose SSD volume that balances price and performance for
a wide variety of workloads
2. iol / io2 (SSD): Highest-performance SSD volume for mission-critical low-latency or
high-throughput workloads
3. stI (HDD): Low cost HDD volume designed for frequently accessed, throughput-
intensive workloads
4. scI (HDD): Lowest cost HDD volume designed for less frequently accessed workloads

- Volume Network Drive (provides low latency access to data)
- Can only be mounted to 1 instance at a time (except EBS multi-attach)
- Bound to an AZ
- Must provision capacity in advance (size in GB & throughput in IOPS)
- By default, upon instance termination, the root EBS volume is deleted and any other attached EBS volume is not deleted (can be over-ridden using DeleteOnTermination attribute)
- To replicate an EBS volume across AZ or region, need to copy its snapshot
- EBS Multi-attach allows the same EBS volume to attach to multiple EC2 instances in the same AZ
> DeleteOnTermination attribute can be updated for the root EBS volume only from the CLI

### Volume Types¶
1. General-Purpose SSD
    - Good for system boot volumes, virtual desktops
    - Storage: 1 GB - 16 TB
    - gp3
        - 3,000 lOPS baseline (max 16,000 - independent of size)
        - 125 MiB/s throughput (max 1000MiB/s - independent of size)
    - gp2
        - Burst IOPS up to 3,000
        - 3 IOPS per GB
        -Max IOPS: 16,000 (at 5,334 GB)

2. Provisioned IOPS SSD
- Optimized for Transaction-intensive Applications with high frequency of small & random IO operations. They are sensitive to increased I/O latency.
- Maintain high IOPS while keeping I/O latency down by maintaining a low queue length and a high number of IOPS available to the volume.
- Supports EBS Multi-attach (not supported by other types)
- io1 or io2
    - Storage: 4 GB - 16 TB
    - Max IOPS: 64,000 for Nitro EC2 instances & 32,000 for non-Nitro
    - 50 lOPS per GB (64,000 IOPS at 1,280 GB)
    - io2 has more durability and more IOPS per GB (at the same price as io1)

- io2 Block Express
    - Storage: 4 GiB - 64 TB
    - Sub-millisecond latency
    - Max IOPS: 256,000
    - 1000 lOPS per GB
   
3. Hard Disk Drives (HDD)
- Optimized for Throughput-intensive Applications that require large & sequential IO operations and are less sensitive to increased I/O latency (big data, data warehousing, log processing)
- Maintain high throughput to HDD-backed volumes by maintaining a high queue length when performing large, sequential I/O
- Cannot be used as boot volume for an EC2 instance
-Storage: 125 MB - 16 TB
- Throughput Optimized HDD (st1)
    - Optimized for large sequential reads and writes (Big Data, Data Warehouses, Log Processing)
    - Max throughput: 500 MB/s
    - Max IOPS: 500
- Cold HDD (sc1)
     - For infrequently accessed data
     - Cheapest
     - Max throughput: 250 MB/s
     - Max IOPS: 250

## Encryption
- For Encrypted EBS volumes
- Data at rest is encrypted
- Data in-flight between the instance and the volume is encrypted
- All snapshots are encrypted
- All volumes created from the snapshot are encrypted

### Encrypt an un-encrypted EBS volume
- Create an EBS snapshot of the volume
- Copy the EBS snapshot and encrypt the new copy
- Create a new EBS volume from the encrypted snapshot (the volume will be automatically encrypted)
> All EBS types and all instance families support encryption but not all instance types support encryption.
