1. create an AKS cluster
2. create a storage account in same RG
3. give contributor role to aks cluster
4. Pass storage account name and rg in storageclass
5. pass storageclass name in volumeClaimTemplate of Statefulset
