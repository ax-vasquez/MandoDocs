# [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
EC2 offers a variety of different instances to facilitate different use cases. Instance types comprise varying combinations of CPU, memory, storage, and networking capacity and give you the flexibility to choose the appropriate mix of resources for your application.

# General Purpose

## T2
General purpose, [Burstable performance instances](https://aws.amazon.com/ec2/instance-types/#burst)
  - They provide baseline level of CPU performance
  - Posses ability to burst above the baseline
- T2 Unlimited instances can sustain high CPU performance for as long as a workload needs
  - For most general-purpose workloads, T2 Unlimited instances will provide ample performance without any additional charges
- Baseline performance and ability to burst are governed by CPU credits
  - T2 instances receive CPU credits continuously at a set rate depending on the instance size
  - CPU credits are consumed when they are active ("CPU burst")
- T2 Instances are a good choice for a variety of general-purpose workloads including:
  - Microservices
  - Low-latency interactive applications
  - Small and medium databases
  - Virtual desktops
  - Development
  - Build and stage environments
  - Code repositories
  - Product prototypes

### T2 Features
- High Frequency Intel Xeon Processors
- Burstable CPU, governed by CPU credits, and consistent baseline performance
- Lowest-cost general purpose instance type, and Free Tier eligible (terms apply)
- Balance of Compute, memory and network resources

## M5
Latest generation of General Purpose instances. This family provides a balance of compute, memory, and network resources, and it is a good choice for many applications.
- M5 instances are ideal for the following use cases:
  - Small and mid-size databases
  - Data-processing tasks that require additional memory
  - Caching fleets
  - Running backend servers:
    - SAP
    - Microsoft SharePoint
    - Cluster computing
    - Other Enterprise applications

### M5 Features
- 2.5 GHz Intel Xeon Platinum 8175 processors with new Intel Advanced Vector Extension (AXV-512) instruction set
- New and larger instance size, m5.24xlarge, offering 96 vCPUs and 384 GiB of memory
- EBS-optimized by default and higher EBS performance on smaller instance sizes
- Up to 25 Gbps network bandwidth using Enhanced Networking
- Requires HVM AMIs that include drivers for ENA and NVMe
- Powered by the new light-weight Nitro system, a combination of dedicated hardware and lightweight hypervisor

## M4
These instances provide a balance of compute, memory and network resources. They are a good choice for many applications.
- M4 instances are ideal for the following use cases
  - Small and mid size databases
  - Data processing tasks that require additional memory
  - Caching fleets
  - Running back end servers for
    - SAP
    - Microsoft Sharepoint
    - Cluster computing
    - Other Enterprise applications

### M4 Features
- 2.3 GHz Intel Xeon E5-2686 v4 (Broadwell) processors
  - OR 2.4 GHz Intel Xeon E5-2676 v3 (Haswell) processors
- EBS-optimized by default at no additional cost
- Support for Enhanced Networking
- Balance of compute, memory, and network resources

# [Pricing](https://aws.amazon.com/ec2/pricing/on-demand/)

## On-Demand Pricing
With this model, _you only pay for the instances you use_. On-Demand instances free you from the costs and complexities of planning, purchasing, and maintaining hardware and transforms what are commonly large fixed costs into much smaller variable costs.
- Pay for compute capacity by the hour or second (minimum of 60 seconds)

## Spot Pricing
With Spot instances, you pay the Spot price that's in effect for the time period your instances are running. _Spot instance prices are set by Amazon EC2 and adjust gradually based on long-term trends in supply and demand for Spot instance capacity_. The pricing data on the AWS site is updated every 5 minutes, so this is a very dynamic value.
- Spot instances are available _at a discount of up to **90%** off_ compared to On-Demand pricing
  - See the Spot Instance Advisor to see [AWS' comparison](https://aws.amazon.com/ec2/spot/bid-advisor/) between Spot and On-Demand pricing
- Spot instances are also available to run for a predetermines duration (in hourly increments up to 6 hours in length) at a discount of up to 30 to 50% compared to On-Demand pricing
