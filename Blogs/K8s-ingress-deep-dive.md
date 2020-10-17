- ingress does not work with ingress controller
  implements the ingress-api. 
  An ingress controller is usually an application that runs as a pod in a Kubernetes cluster and configures a load balancer according to Ingress Resources.
  Ingress Controller is an application which captures incoming requests and routes them according to rules mentioned in the Ingress resource.
- how rules from ingress are passed to ingress controller 
- can we use one ingress controller for multiple namespaces
- how do we use multiple ingress controllers


This means, by default, each Ingress controller will listen to all the ingress events from all the namespaces and add corresponding directives and rules into Nginx configuration file.

Demo steps
- create in ingress controller 
- create two services 
- create an ingress resource 
- open nginx config file - show that this file has config rule mentioned in ingress resource 
- create another ingress resource in other namespace 
- check if these are updated in ingress controller 

https://itnext.io/kubernetes-ingress-controllers-how-to-choose-the-right-one-part-1-41d3554978d2