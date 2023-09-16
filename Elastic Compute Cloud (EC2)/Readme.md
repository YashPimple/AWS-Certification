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
- EBS volumes are network drives with good but "limited" performance
- If you need a high-performance hardware disk, use EC2 Instance Store
- Better I/O performance
- EC2 Instance Store lose their storage if they're stopped (ephemeral)
- Good for buffer / cache / scratch data / temporary content
- Risk of data loss if hardware fails
- Backups and Replication are your responsibility 
