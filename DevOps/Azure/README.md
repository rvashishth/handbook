https://status.azure.com/en-us/status

### Traffic routing/networking

- Azure Load Balancer
  - layer 4
  - 5 tuple hash source ip, source port, destination ip, destination port and protocol
- Azure Application Gateway
  - layer 7
  - web traffic load balancer, route request based on layer 7 url. i.e. /gateway-ui, /duplo
- Azure API Management
- Azure Traffic Manager
- Azure Front Door

- [ ] create a network diagram of Azure TF -> App Gateway -> ALB
