# Infra Estimation
- [Infra Estimation](#infra-estimation)
  - [Overview](#overview)
  - [Disk Storage Size](#disk-storage-size)
  - [Pods Resources (Broker & Bookie)](#pods-resources-broker--bookie)
  - [VM](#vm)
  - [Performance Test Results](#performance-test-results)

## Overview

This document intent to evaluate & estimate required infrastructure for pulsar cluster, basis on the workload required workload capabilities.

## Disk Storage Size

## Pods Resources (Broker & Bookie)

Following two tables are derived from [streamnative cloud](https://console.streamnative.cloud) create cluster page.

| Bookie Size                                | Pod Count | Performance                                 |
| ------------------------------------------ | --------- | ------------------------------------------- |
| 0.2 CPU </br> 256Mi Memory </br> 16Gi Disk | 2         | throughput 4 Mbps, rate 400 entries/sec     |
| 1 CPU </br> 1Gi Memory </br> 256Gi Disk    | 2         | throughput 17.5 Mbps, rate 1750 entries/sec |
| 1 CPU </br> 2Gi Memory </br> 512Gi Disk    | 2         | throughput 35 Mbps, rate 3500 entries/sec   |
| 2 CPU </br> 4Gi Memory </br> 1Ti Disk      | 2         | throughput 70 Mbps, rate 7000 entries/sec   |
| 4 CPU </br> 8Gi Memory </br> 2Ti Disk      | 2         | throughput 150 Mbps, rate 15000 entries/sec |


| Broker Size               | Pod Count | Performance                                              |
| ------------------------- | --------- | -------------------------------------------------------- |
| .2 CPU </br> 256Mi Memory | 1         | write 3 Mbps, read 6 Mbps, write rate 350 entries/sec    |
| 2 CPU </br> 1Gi Memory    | 1         | write 12 Mbps, read 24 Mbps, write rate 1250 entries/sec |
| 2 CPU </br> 2Gi Memory    | 1         | write 25 Mbps, read 50 Mbps, write rate 2500 entries/sec |



If batching at producer, use throughput for estimating the capacity. For example, if producer is sending 100 msg/s with each msg of 5kb, you are sending 500 KB/s.

If batching is disabled, use entry rate for estimating the capacity. In this case, one message is an entry. If you are sending 100 msgs/s, then the entry rate is 100 entries/s.

## VM

Following are the parameters to consider while selecting a VM size

- IOPS(input/ouput per second) rate for data supported by per VM
- Max disk attachment supported by VM
- Memory required to run all pods(mainly max limits for bookie/brokers)
- CPU required to run all pods(mainly max limits for bookie/brokers)

IOPS required for 1 TB/hour ~1160  where one 

> 256kb read/write is considered as 1 IOPS in azure

## Performance Test Results

**Test 1**

1 topic - 1 partition - 1 consumer/1 producer - 1 kb - 5000 msg/sec </br>
AKS Cluster 3 node(DsV2 - 2 cpu, 7 gb ram)

Duration 5 min

Aggregated End To End Latency Avg 38 ms

CPU utilization was around 70% 

