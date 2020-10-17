- [az login](#az-login)
- [az account](#az-account)
- [az aks](#az-aks)
- [az ad](#az-ad)

### az login

### az account

- `az account list-locations -o table`
- `az account list` show all subscription
- `az account show` show subscription which is currently in use
- how to switch subscription `az account set --subscription=af030d48-2dc2-434f-8a9d-737be02070bb`

### az aks

- how to login to azure aks cluster
  ```
  $az aks get-credentials --resource-group=rg_name --name=aks_cluster_name
  $az aks get-credentials --resource-group=link-platform-pr-23 --name=link-platform-pr-23-k8s
  ```
  now you can browse the cluster using `kubectl` commands
- how to check k8s supported versions in AKS </br>
  `$ az aks get-versions --location centralus --output table`
- How to browse K8s web console from cloud shell or cli in local</br>
  `az aks browse`
- how to open k8s dashboard via cloud shell or az login </br>
  login to azure cli or on shell.azure.com </br>
  `az login`
  get k8s cluster credentials on azure cli </br>
  `az aks get-credentials --name pulsar-pr-2-k8s --resource-group pulsar-pr-2`
  open a bridge to browse k8s web console </br>
  `az aks browse --resource-group rg_in_which_aks_is --name aks_cluster_name`

### az ad

- `az ad sp show --id application_client_id_from_portal_of_an_AD_app` we can get object_id and other details for this app.
