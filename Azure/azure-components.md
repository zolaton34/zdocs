# [Core Azure architectural components](https://docs.microsoft.com/en-us/learn/modules/azure-architecture-fundamentals/)

## Subscriptions, management groups, and resources
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
