- [How terraform client login on azure using jenkins](#how-terraform-client-login-on-azure-using-jenkins)
- [Create an SP for AKS which allow AKS to create roles](#create-an-sp-for-aks-which-allow-aks-to-create-roles)
- [AKS Service Load Balance with Public IP and DNS Name](#aks-service-load-balance-with-public-ip-and-dns-name)

#### How terraform client login on azure using jenkins

Make use of service principle(Got Azure AD > App Registration). And then create an credential in

Jenkins Azure Ref - https://plugins.jenkins.io/azure-ad/

| Azure                                            | Jenkins page    |
| ------------------------------------------------ | --------------- |
| azure subscription id                            | subscription_id |
| ad app - application/client id                   | client id       |
| ad app directory/tenant id                       |                 |
| object id                                        |                 |
| active directory id                              | tenant id       |
| ad app - certificate and secrets - client secret | client secret   |

The secret can be read only once while creating it.

#### Create an SP for AKS which allow AKS to create roles

For documentation on Roles, Navigate to Azure -> Role Base Access Control

Assign a role to -https://docs.microsoft.com/en-gb/azure/role-based-access-control/role-assignments-portal

Create custom role - https://docs.microsoft.com/en-gb/azure/role-based-access-control/custom-roles

Each SP can have one or more Role
Each role can have one or more Permission

Security principle = user principle/service principle

Why we need service principle with AKS - https://docs.microsoft.com/en-us/azure/aks/kubernetes-service-principal

> when creating an aks cluster using `az aks` command it also generates a SP, but to create an AKS cluster using terraform, we must first create an SP

> the object id for an app/sp displayed at azure portal is different then the object id returned using azurecli command `az ad sp show --id application_client_id_from_portal` use this object id as principle_id in terraform **azure_role_assignment**

**how to get password for a service principle to use with aks** - password can only be retrieved while creating the service.

> **Run your terraform with a SP that has access to do role assignments, the AKS SP will be contributor.**

How to manually assign a role to the SP of aks

1. Navigate to Azure AD > app registration.
2. Search by AKS cluster name, copy SP name
3. Open resource group
4. Click on Access Control IAM, Add a role assignment.
5. Enter AKS SP name,

#### AKS Service Load Balance with Public IP and DNS Name

Reference - https://docs.microsoft.com/en-in/azure/aks/static-ip#apply-a-dns-label-to-the-service

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-dns-label-name: myserviceuniquelabel
  name: azure-load-balancer
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

- This will create a dynamic external public ip
- External load balancer
- A http dns url with location name http://rahulvotepoc.centralus.cloudapp.azure.com/
- **Users can also access this app using the external public ip**

- [ ] how we can block app/service access using external ip and make it compulsory to use dns name only

### Login to AKS k8s dashboard

https://docs.microsoft.com/en-us/azure/aks/kubernetes-dashboard

After kubernetes version 1.16, kubernetes-dashboard cluster role binding is not required, instead login using kube config file

If it still don't work do following </br>
https://github.com/Azure/AKS/issues/1573#issuecomment-628937398

### Running a docker image in AKS

1. create acr and aks, create a role assignment for aks to pull image from acr
2. login to acr
   `az acr login -n acrname -g rg-name`
3. get pr login server
4.
