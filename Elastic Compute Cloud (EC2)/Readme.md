- Regional Service
- EC2 (Elastic Compute Cloud) is an Infrastructure as a Service (IaaS)
- Stopping & Starting an instance may change its public IP but not its private IP
- AWS Compute Optimizer recommends optimal AWS Compute resources for your workloads
- There is a vCPU-based On-Demand Instance soft limit per region

EC2 Instances Purchasing Options
• On-Demand Instances - short workload, predictable pricing, pay by second
• Reserved (I & 3 years)
• Reserved Instances - long workloads
• Convertible Reserved Instances - long workloads with flexible instances
• Savings Plans (I & 3 years) -commitment to an amount of usage, long workload
• Spot Instances - short workloads, cheap, can lose instances (less reliable)
• Dedicated Hosts - book an entire physical server, control instance placement
• Dedicated Instances - no other customers will share your hardware
• Capacity Reservations - reserve capacity in a specific AZ for any duration

EC2 Spot Instance Requests
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

Spot Fleets
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
