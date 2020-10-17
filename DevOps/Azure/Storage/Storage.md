- [Azure Storage](#azure-storage)
  - [Azure FileShare](#azure-fileshare)
  - [Ref](#ref)
- [Disk](#disk)
- [Kubernetes](#kubernetes)
  - [volumes](#volumes)
  - [persistent volumes](#persistent-volumes)
  - [PersistentVolumeClaim](#persistentvolumeclaim)
  - [volume attachments](#volume-attachments)
  - [StatefulSets](#statefulsets)
    - [Ref](#ref-1)
  - [StorageClass](#storageclass)
  - [Steps to create stateful sets](#steps-to-create-stateful-sets)
- [TODO for pulsar](#todo-for-pulsar)
  - [Azure](#azure)
  - [StorageClass configs](#storageclass-configs)
- [Question](#question)
- [Rough](#rough)
- [User story](#user-story)
  - [Why we need to do this](#why-we-need-to-do-this)
  - [UseCase 1: ReclaimPolicy for StorageClass and PV </br>](#usecase-1-reclaimpolicy-for-storageclass-and-pv-br)
    - [Questions](#questions)
  - [UseCase 2: test pulsar infra. does it re-use same pv or creates new](#usecase-2-test-pulsar-infra-does-it-re-use-same-pv-or-creates-new)
  - [UseCase 3: Test Statefulset](#usecase-3-test-statefulset)
  - [Implementation:-](#implementation-)

## Azure Storage

- Azure Disks are mounted as ReadWriteOnce, so are only available to a single pod.
- For storage volumes that can be accessed by multiple pods simultaneously, use Azure Files.
- If multiple pods need concurrent access to the same storage volume, you can use Azure Files to connect using the Server Message Block (SMB) protocol
- reclaimPolicy - for a storage class define

New azurefile-csi driver for azure - https://github.com/kubernetes-sigs/azurefile-csi-driver </br>
Any azure storage issue, ask https://github.com/andyzhangx/demo/

### Azure FileShare

- minimum size should be 100gb for premium azurefile
- FileShare - PV - PVC has 1-1 mapping

### Ref

- https://docs.microsoft.com/en-us/azure/aks/concepts-storage
- https://docs.microsoft.com/en-us/azure/aks/operator-best-practices-storage
- https://docs.microsoft.com/en-us/azure/aks/azure-files-dynamic-pv

## Disk

- Disk can only be used with VM
- 

## Kubernetes

### volumes

Last as long as the pod. A pod can have multiple volumes

### persistent volumes

A persistent volume represents a piece of storage that has been provisioned for use with Kubernetes pods.
A persistent volume can be used by one or many pods, and can be dynamically or statically provisioned.

### PersistentVolumeClaim

A request for a PV, and we attach PV to Pods via PVC. As a pod consume node resources like cpu and ram, a PVC consume PV resources.
A PVC with accessMode: ReadWriteOnce allow only one node to use this PVC.
A persistent volume claim (PVC) is used to automatically provision storage based on a storage class.

> Once the PVC has claimed the PV, it will be available for use in the enclosing Pod as a mounted Volume.
> We can define storage class name for both PVC and PV, so the PVC will only bind to the PV which is having same storage class name.

### volume attachments

### StatefulSets

Like a deployment(for stateless services) StatefulSet manage deployment but for stateful apps. i.e. DB, Zookeeper, Bookkeepr

Every replica of a stateful set will have its own state, and each of the pods will be creating its own PVC(Persistent Volume Claim). So a statefulset with 3 replicas will create 3 pods, each having its own Volume, so total 3 PVCs.

Like we use volume in deployments we need to use **volumeClaimTemplates** for StatefulSte. With one StatefulSet we can use multiple **volumeClaimTemplates**. When this is used, no need to define PVC spec file

the persistent Pod identifiers make it easier to match existing volumes to the new Pods that replace any that have failed.

StatefulSet create ordered stable naming for pods(even after restarts) and pvc, so if zookeeper-0 is created as master, after each restart it remains as master. It allow attaching same persistent disk to a pod even if it gets rescheduled to a new node

#### Ref

- [How each replica pod keeps it's own pvc](https://medium.com/stakater/k8s-deployments-vs-statefulsets-vs-daemonsets-60582f0c62d4)
- [How pod name and order, network id works, postgresql example](https://medium.com/yugabyte/orchestrating-stateful-apps-with-kubernetes-statefulsets-ce3a4a9dfd7e)
- [volumeClaimTemplate accessModes](https://medium.com/@zhimin.wen/persistent-volume-claim-for-statefulset-8050e396cc51)
- [updateStrategies](https://medium.com/velotio-perspectives/exploring-upgrade-strategies-for-stateful-sets-in-kubernetes-c02b8286f251)

### StorageClass

Dynamically provision a storage resource.

The reclaim policy Delete indicates that the underlying Azure File Share is deleted when the persistent volume that used it is deleted.

### Steps to create stateful sets

Step 1. Create a StorageClass resource spec file </br>
Step 2. Create a PVC resource spec file, which will refer to StorageClass resource </br>
OR </br>
Step 2. Define volumeClaimTemplates in StatefulSet resource file </br>

## TODO for pulsar

### Azure

- create a storage account
- create a file share
-

### StorageClass configs

- Update Storage Class
  - provisioner: kubernetes.io/azure-file
  - reclaimPolicy: Retain
  - StorageClass use azurefiles premium
  - add updateStrategy(rolling update) for bookkeeper and zookeeper stateful set

## Question

- by default which storage Class is used by pulsar? azure disks or azure files? and which reclaimPolicy is used? </br>
  **Answer:** all PVC for bookie, prometheus, zookeeper are using default(disk standard hdd) storage class

- ReadWriteOnce is defined as access mode in volumeClaimTemplates for bookkeeper stateful set, do we need to change this?
- what is the k8s value for provisioner for azurefile-premium storage class
- do i need to create the storage first before installing pulsar charts or with the charts?
- the choice of Disks or Files is often determined by the need for concurrent access to the data or the performance tier.
- Azure Storage Account? how does it relate to Azure Disks or Azure Files
- If PV can use both azure disk and azure files, then how disks can only be used with a node
- If stateful set is required to horizontally scale deployment with PVC then why we don't use StatefulSet with prometheus pods in pulsar charts
- How to do blue/green deployment for k8s stateful sets. As we want one to one association between pod and pvc. How does the pods works with old pvc
- which updateStrategy should be used with StatefulSet
- Why AKS has created four storage classes with a default one. how can i change the default storage class in AKS, is it possible via TF
- do i need to create an storage account in advance in same resource group

Azure Storage Account Related Questions

- Can i have multiple FileShare storage under one storage account, or i need to create multiple storage account
- Do i still need to use StorageClass resource if i create storage account in advance
- Can the storage account dynamically expended
- [x] Does dojo have a module for storage account </br>
      **Answer:** yes. use dojo storage profile

## Rough

Provisioner: kubernetes.io/azure-file
Parameters: skuName=Premium_LRS
AllowVolumeExpansion: True
MountOptions: <none>
ReclaimPolicy: Delete
VolumeBindingMode: Immediate
Events: <none>

## User story

### Why we need to do this

- re-creating/removing the AKS cluster will remove the storage account, created by the default storage classes
- how do we expand/autoexpand storage for pods
- re-use existing pv for new re-created prometheus pods
- how do we limit the max storage

### UseCase 1: ReclaimPolicy for StorageClass and PV </br>

When the reclaimPolicy: Delete for a StorageClass. With this setting, as soon as a PersistentVolumeClaim is deleted, it also triggers the removal of the corresponding PersistentVolume along with the Azure Disk.

**Test 1** - use (default azurefile)StorageClass(reclaimPolicy: Delete) and dynamic PV with default(Delete) reclaimPolicy</br>

- created pv, a storage account, a file share in storage account
- one file share for 1 PV is created, use the storage account if it already exist(in same region, same type)
- the auto created storage account will have all storage types, fileshare, blob,
- on deleting the resources, it has deleted the PV, PVC, and fileShare
- did not delete the storage account

**Test 2** - Custom StorageClass with(reclaimPolicy: Retain) of azurefile provisioner type and dynamic PV

- PV created using this StorageClass will also have the same reclaim policy Retain
- Creating resource will create PV, StorageClass, PVC, Azure Storage Account and a fileshare
- deleting resources will delete everything except Storage Account and fileshare
- **Creating resource again creates a new PV instead of using the existing PV**

#### Questions

1. How can we use existing PV for new/re-deployments. Because a PV retained from previous charts doesn't get associated with new Pod(probably using statefulset)

### UseCase 2: test pulsar infra. does it re-use same pv or creates new

- PVC and PV created for zookeeper and bookie were not deleted, after uninstalling the chart
- when re-creating/re-installing chart, previous pods get attach to the previous pv, pvc
- reclaim policy is delete for default pulsar-infra bookie and zookeeper pods

**Test 1** - if we create a custom class with autoexpension, and reclaim policy is set to Retain. Can we still re-associate old pv(of bookie and zookeeper) with new replaced/recreated pods?

### UseCase 3: Test Statefulset

- a pvc is not required when using statefulset
- statefulset creates the pv, pvc basis on the storageclass name defined in `volumeClaimTemplate`
- a PV, PVC and azurefile/disk is not deleted when we delete the pod or statefulset
- once we delete AKS cluster, and re-create it again it creates new PV. Ask in team if we should provide pre-provisioned PV? otherwise re-creating a cluster will result in loss of data.

**Test** Use externally created PV with StatefulSets, the PV must either be provisioned by admin or using PV provisioner(automatic), do we have to create PV, StorageClass, StorageAccount in advance

**Test** - is it good idea to have statefulset for prometheus, 1) benefit - it helps to reassociate a new prometheus pod with old pv, so no data loss on pod restart 2) what if we have two pod-two pv, is there any issue with data

**Test** - how zookeeper manage state
**Test** - Test Zookeeper and bookie pods with PV AccessMode set to ReadWriteMany

- can we use azure disk, and wire up these disk with PV. </br>
  We can't use disk is because they associate with one node at a time.(ReadWriteOnce -- the volume can be mounted as read-write by a single node) with that if we needs to reschedule the zookeeper or bookie nodes they won't be able to associate with same PV.

Created an issue - https://github.com/andyzhangx/demo/issues/8

Test 1 - increase the pod count in prometheus deployment, check if it does create new PV for additional Pods
Test 2 - test Statefulset with custom storage class, if it creates dynamic PV
Test 3 - test, test 2 configs with zookeeper

### Implementation:-

- create StorageAccount using dojo tf
- create a storage account with all type of storage in it, like the one we create using portal
- create StorageClass using kubernetes tf provider(pass rg, storageAccount name)
- Pass the storage class name in helm-release
- define storageClass name in `volumeClaimTemplate` of statefulset
- azurefile-premium for prod and azurefile for nonprod storage type
- dynamic pv creation with custom storage class and external storage account(this pv should not be deleted and should be reattached to the pods again)
- create statefulset for prometheus
- add comment - as bryce suggested - LIkewise please use the bootstrap storage accounts for bootdiag, backup, etc.
