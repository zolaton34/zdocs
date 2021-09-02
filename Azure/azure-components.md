# [Core Azure architectural components](https://docs.microsoft.com/en-us/learn/modules/azure-architecture-fundamentals/)

## [Subscriptions, management groups, and resources](https://docs.microsoft.com/en-us/learn/modules/azure-architecture-fundamentals/overview)
Top-down hierarchy:
* __Management group__
    * To manage access policy, and compliance for multiple subscriptions
    * Subscriptions are included in a management and inherit its conditions
* __Subscription__
    * Groups together user accounts and the resources created by them
    * Has limits and quotas
    * To manage costs and resources created by an user, a team or a project
* __Resource groups__
  * Logical container where Azure resources are deployed and managed
* __Resources__
  * Instance of services
    * Virtual machines
    * Storage
    * SQL databases
    * etc.

## [Regions, availability zones, and region pairs](https://docs.microsoft.com/en-us/learn/modules/azure-architecture-fundamentals/regions-availability-zones)
### Azure region
* Geographical area
* One or more datacenter
  * Nearby
  * Networked by low-latency network
* Generally we need to choose region when deploying a resource
  * But some exceptions like Azure DNS etc.
* Some services only available in some regions

### Special Azure regions
For compliance or legal purpose.

### Azure availability zones
* For redudancy
* Physically separate datacenters within an Azure region
* High-speed connected
* [Not all regions support availability zones](https://docs.microsoft.com/en-us/azure/availability-zones/az-region)
* Service categories in availability zones
  * __Zonal service__ Pinned to a specific zone
  * __Zone-redundant service__ Resource replicated accross zones
* There is st least three zones in a region

### Regions pairs
* Each region paired to another region
* In the same geography (example US, Europe, Asia etc.)
* At least 300 km one from another
* Resources replication and failover between paired regions
![Region pairs](https://docs.microsoft.com/en-us/learn/azure-fundamentals/azure-architecture-fundamentals/media/region-pairs-d9eb9728.png)