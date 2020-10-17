# Database Per Service

1. [Why we need it](#why-we-need-it)
2. [Issue and Consideration](#issue-and-consideration)
3. [Best Approach](#best-approach)
4. [Related Design Patterns](#good-to-know)
5. [References](#references)

## Why we need it
- Database should be owned by the entity- to maitain data integrity
- Scaling
- Work with different types of db
- Deployment, low downtime 
- Reduce db level complexity join etc
- Cost effective
  
## Issue and Consideration
- Only AP can be achieved through this. Consistency is achieved only eventually.
- Overall maintenance
- Increased Learning curve
- Increased Complexity at Services Integration

  
## Best Approach
Do db Separation based on domain needs  
- Table level using ACL(Access Control List) where only separation of concern is important 
- Schema level where resource contraint is required
- DB Server where we need scaling and partitioning is required
  
## Good To Know
- CAP Theorem
- Domain Driven Design
- Bounded Context
  
### References
- https://microservices.io/patterns/data/database-per-service.html
