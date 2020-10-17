we can use  multiple approach for authentication
    tls 
    jwt 

while most of the  people use tls for data  encryption

mutual tls is  for  authentication but not for  encryption 

client side certificate is only required for mutual tls. 

#### Questions 
- Keystore - stores the public key and private key. does it store the other parties public key?
- truststore - why do we need to store  other parties certificate? 
- private keys are required to  sign new certificates - if i have a private key can i create/sign multiple certificates?
- why do we need to distribute  the public cert to all connecting parties? why does 3rd parties  need this cert to connect to the original cert  owner service?
- for mutual tls, does the server has capability create new cert for clients?

#### Pulsar questions 
- enable tls encryption for broker in proxy, does the tls terminates at proxy?
- if we enable encryption at proxy, does proxy needs to generate a cert for each client? does each producer consumer needs to get a cert from proxy?


Pulsar TODOs
- we will need to enable keystore and truststore for bookkeeper and zookeeper for encryption and authentication
- secure  every connection between all components i.e. broker, bookkeeper, zookeeper, function worker
- we can do tls encryption at two levels, one between broker/proxy and clients. and second is between all  pulsar components