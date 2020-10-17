
# Messaging in Azure: Service Bus, Event Grid, Event Hubs
- [Messaging in Azure: Service Bus, Event Grid, Event Hubs](#messaging-in-azure-service-bus-event-grid-event-hubs)
  - [Azure Service Bus: Premium](#azure-service-bus-premium)
    - [Question](#question)
    - [Cost](#cost)
    - [Features:](#features)
    - [Limitations:](#limitations)

## Azure Service Bus: Premium 

### Question
1. What max message rate/throughput a premium message unit can support? https://stackoverflow.com/questions/64274946/what-is-max-throughput-and-message-rate-a-message-unit-can-support-in-azure-serv


### Cost 
Hourly	$0.928/hour Per message unit


Brokered connections are not charged in the premium tier.

### Features:
Geo Disaster Recovery Support using Additional Service Bus Namespace

Service Bus premium runs in dedicated resources to provide higher throughput and more consistent performance.

Message unit auto-scaling

### Limitations:
Partitioned queues and topics aren't supported in Premium Messaging.

Max message size 1mb

Max topic/queue size is 5gb
