# Azure Disk
- [Azure Disk](#azure-disk)
  - [Overview](#overview)
    - [Backup](#backup)
    - [Encryption](#encryption)
  - [Open Questions](#open-questions)
    - [Disk](#disk)
    - [Backup](#backup-1)
  - [Jake](#jake)
    - [Things to explain to jake](#things-to-explain-to-jake)
    - [Questions To Ask](#questions-to-ask)
  - [Question by Jake](#question-by-jake)

## Overview 

This document covers azure disk, it's backup and data encryption with Disk usage with Kubernetes

Azure Disk Types</br>
  1. Ultra Disk (max size 65 TB, throughput  2GB)
  2. Premium SSD (max size 32 TB,  throughput 900mb)
  3. Standard SSD
  4. Standard HDD

Azure Disk Role Types </br>
1. Data Disk (max size 32TB)
2. OS Disk (max size 4TB)
3. Temporary Disk ()

Managed disk keeps 3 replica of data for high durability

Only premium disk guarantee the performance, throughput and IOPS.

IOPS - Input output operation per second, where 256kb read/write is considered as one operation

You can attach a number of data disks to an Azure virtual machine.

Premium storage account has a maximum total throughput rate of 50 Gbps. The total throughput across all of your VM disks should not exceed this limit.

We might need to create multiple storage accounts if throughput need is very high

> max 16 disk volume can be added to one k8s node

An Azure disk can only be mounted with Access mode type ReadWriteOnce, which makes it available to only a single pod in AKS.

**Unmanaged disks** require that you create a storage account to store the disks. disks and are stored in containers in the storage account.

Ultra Disk for AKS are still in preview mode. AKS not yet supports the ultra disk

### Backup

Supports max 32TB disk size

Snapshot vs Image - a snapshot is copy of just one disk, while the image is a copy of entire VM including all disk.

Use Velero

Disk Backup/Recovery

Backup and test the restore process for storage

Storage that connect to individual pod 

Each VM has max disk count that can be attached to VM, max IOPS supported by a 

Regular backup/snapshot

Azure Disks can use built-in snapshot technologies

Persistent volumes follow pods even if the pods are moved to a different node inside the same cluster.

- Is it good idea to take backup of pulsar data
  I guess not, we should not keep data on topic for long, may be an hour or so. 
  Move data to tiered storage if required longer then that. 


Search
- data backup/replication for streaming data store  
  
https://docs.microsoft.com/en-gb/azure/aks/operator-best-practices-multi-region#infrastructure-based-asynchronous-replication

### Encryption

**Server Side Encryption** (SSE), which is performed by the storage service.

> Service Side Encryption is enabled by default

**Azure Disk Encryption** (ADE), which you can enable on the OS and data disks for your VMs

## Open Questions

### Disk
- [ ] VM Scale set vs availability set
- [ ] Does azure keep disk replica in different Availability Zones? or within same zone.
- [ ] How Disk supports availability zone
### Backup
- [ ] Does backup only required to protect against regional disaster
- [ ] Do we need to take snapshots, how snapshots are different then backups?

## Jake

Jake Discussion

  AKS creates 4 storages classes, default one creates azure disk of type Standard SSD(LRS)
  This allow statefulset, to dynamically create disk using PVC. AKS creates those disk in the shadow AKS resource group

  So for any reason AKS cluster is re-created we will also loos the disk storage.

### Things to explain to jake 
  - Zookeeper storage. Each node in cluster have one copy
  - Zookeeper How auto-scaling works
  - Bookkeeper - one ledger is stored across multiple bookie
  - Bookkeeper - how auto-scaling can work
  - Stateful Set, PVC, dynamic creation of disk

### Questions To Ask
  - If it is a good idea, should we plan to scale Bookkeeper/Zookeeper storage pods? and how horizontal or vertical storage?
  - Stateful Set, how we can possibly use existing disk
  - Should we create PV, PVC, Managed Data Disk in advance. If we do how we should scale up/down?
  - Disk storage reservation
  - Disk already have default encryption for data at rest, is that enough or we should look for different encryption

## Question by Jake
- is there a way to replicate storage outside of the pulsar RG? to some cold-storage or something


subhadh school - n