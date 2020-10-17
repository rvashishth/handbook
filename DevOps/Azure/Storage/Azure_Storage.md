Storage account
- we can select which network can access storage account
- Storage account provides Azure Storage Service Encryptions
- 

Disk 
- VM uses virtual disk
- Virtual Hard Disk are stored as page blobs in storage accounts
- managed vs unmanaged disk 
- ephemeral disks
- Disk role - OS Disk, Data Disk, Temporary Disk 
- Data Disk = we can add one more more data virtual disk to each VM 
- Managed Disk = stores three replica of data 
- Managed Disk = are stored as page blobs 
- Managed Disk = we can use SSE(Azure Storage Service Encryption) or Azure Disk Encryption
- Managed Disk = Azure Backup natively supports managed disk
- Can't use Ultra SSD = supports max 64TB capacity, can only be attached to ES/DS v3 VMs, No encryption, no backup
- SSD are of two types - Premium SSD and Standard SSD
- For guaranteed throughput always use premium SSD.  Standard SSD/HDD does not guarantee the throughput

Action Items for SA 
- we might need two storage account, 1 for tiered storage another for hot/disk storage
- we need 1 standard storage account for performance test results
- we might not need geo-redundant storage
- account type should be general purpose v2
- access tier hot is enough for both storage account
- routing preference is default, microsoft network routing
- use a virtual network
- Disk, use data disk, premium SSD

Questions 
- do we need a SA to use disk
- do we need to create PV and PVC in advance or dynamic 
- if we create disk in advance, will the PV, PVC use these disk as storage
- should i select a vnet for storage account(how will i allow access to ACI from outside of vnet)
- blob vs containers 
- Disk - how backup managed data disk 
- Disk - can we decide which storage account should be used by Data Disk
- Disk - can disk space be scaled up/down on demand
- Storage Account - does standard storage account supports standard SSD or just standard HDD?
- Which Type of disk storage streamnative chart uses?
- Can we plan backup for disk created using dynamic PVC provisioning

K8s Storage Questions
- when uninstall chart, disk should remain there


