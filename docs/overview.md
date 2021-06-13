---
id: overview
title: Welcome to OpenEBS Documentation
sidebar_label: Overview
---
------

## Overview

OpenEBS helps Developers and Platform SREs easily deploy Stateful Workloads in Kubernetes, using Container Attached Storage Engines (aka Data Engines) that are easy to use and can be tailored to run on any kubernetes platform and storage devices of your choice. OpenEBS Data Engines can be used to provide local or distributed block storage, durable and/or fast storage.

<img src="/docs/assets/svg/openebs-architecture.svg" alt="OpenEBS High Level Architecture" style="width:100%">

<br>


### What is OpenEBS?

- OpenEBS is the leading open-source example of a category of cloud native storage solutions sometimes called [Container Attached Storage](https://www.cncf.io/blog/2020/09/22/container-attached-storage-is-cloud-native-storage-cas/). 
- OpenEBS is the most widely deployed and easy to use open-source cloud native storage solution for Kubernetes Stateful workloads. Check out what users of OpenEBS have to say about their experience in <a href="https://github.com/openebs/openebs/blob/master/ADOPTERS.md" target="_blank">OpenEBS Adoption stories</a>.
- OpenEBS is a collection of Storage Engines, allowing you to pick the right storage solution for your Stateful workloads and the type of Kubernetes platform.  At a high-level, OpenEBS supports two broad categories of volumes - [Local Volumes](#local-volumes) and [Replicated Volumes](#replicated-volumes). 
- OpenEBS is a Kubernetes native hyperconverged storage solution that manages the local storage available to nodes and provides local or highly available distributed persistent volumes to Stateful workloads. An added advantage of being a completely Kubernetes native solution is that administrators and developers can interact and manage OpenEBS using all the wonderful tooling that is available for Kubernetes like kubectl, Helm, Prometheus, Grafana, Weave Scope, etc.
- <a href="https://github.com/openebs/openebs/" target="_blank">OpenEBS Project</a> is an open source container attached storage solution originally built by [MayaData](https://mayadata.io). OpeneBS was donated to the _Cloud Native Computing Foundation_ and is now a [CNCF sandbox project](https://www.cncf.io/sandbox-projects/).


### Why do users prefer OpenEBS?

The <a href="https://github.com/openebs/openebs/blob/master/ADOPTERS.md" target="_blank">OpenEBS Adoption stories</a>, mention the top reasons driving users towards OpenEBS as:
- OpenEBS can be used across all Kubernetes distributions - On-premise and Cloud.
- OpenEBS with Kubernetes increases Developer and Platform SRE Productivity.
- OpenEBS is Easy to use compared to other solutions.
- OpenEBS has Excellent Community Support.
- OpenEBS is completely Open Source and Free.

### What does OpenEBS do?

OpenEBS manages the storage available on the Kubernetes nodes and uses that storage to provide Local or Distributed Persistent Volumes to Kubernetes Stateful workloads. 

In case of [Local Volumes](#local-volumes): 
  - OpenEBS can create Persistent Volumes using raw block devices or partitions, or using sub-directories on Hostpaths or by using LVM,ZFS,sparse files. 
  - The local volumes are directly mounted into the Stateful Pod, without any added overhead from OpenEBS in the data path.
  - OpenEBS provides additional tooling for Local Volumes for monitoring, backup/restore, disaster recovery, snapshots when backed by ZFS or LVM, and more. 

In case of [Distributed (aka Replicated) Volumes](#replicated-volumes):
  - OpenEBS creates a Micro-service for each Distributed Persistent volume using one of its engines - Mayastor, cStor or Jiva. 
  - The Stateful Pod writes the data to the OpenEBS engines that synchronously replicates the data to multiple nodes in the cluster. OpenEBS engine itself is deployed as pod and orchestrated by Kubernetes. When the node running the Stateful pod fails, the pod will be rescheduled to another node in the cluster and OpenEBS provides access to the data using the available data copies on other nodes.
  - The Stateful Pods connect to the OpenEBS Distributed Persistent volume using iSCSI (cStor and Jiva) or NVMeoF (Mayastor).
  - OpenEBS cStor and Jiva focus on ease of use and durability of the storage. They use customized versions of ZFS and Longhorn technology respectively for writing the data onto the storage. OpenEBS Mayastor is the latest engine that has been developed with durability and performance as design goals and efficiently manages the compute (hugepages, cores) and storage (NVMe Drives) for providing fast distributed block storage. 

  :::tip NOTE
  OpenEBS Distributed Block volumes are called as Replicated Volumes, to avoid confusion with traditional distributed block storages that tend to distribute the data across many nodes in the cluster. Replicated volumes have low blast radius compared to traditional distributed block storages. Replicated volumes are designed for Cloud Native stateful workloads that require large number of volumes with capacity that can typically be served from a single node as apposed to a single large volume with data sharded across multiple nodes in the cluster.
  :::
 

## Local Volumes 

Local Volumes are accessible only from a single node in the cluster. Pods using Local Volume have to be scheduled on the node where volume is provisioned. Local Volumes are typically preferred for distributed workloads like Cassandra, MongoDB, Elastic, etc that are distributed in nature and have high availability built into them. 

Depending on the type of storage attached to your Kubernetes worker nodes, you can select from different flavors of Dynamic Local PV - Hostpath, Device, LVM, ZFS or Rawfile.

### Local Volume Quickstart Guides

Installing OpenEBS in your cluster is as simple as running a few `kubectl` or `helm` commands. Here are the list of our Quickstart guides with detailed instructions for each storage engine.

- [Local PV hostpath](/docs/next/uglocalpv-hostpath.html)
- [Local PV device](/docs/next/uglocalpv-device.html)
- [ZFS Local PV](https://github.com/openebs/zfs-localpv)
- [LVM Local PV](https://github.com/openebs/lvm-localpv)
- [Rawfile Local PV](https://github.com/openebs/rawfile-localpv)

## Replicated Volumes

Replicated Volumes as the name suggests, are those that have their data synchronously replicated to multiple nodes. Volumes can sustain node failures.  The replication also can be setup across availability zones helping applications move across availability zones. 

Replicated Volumes also are capable of enterprise storage features like snapshots, clone, volume expansion and so forth. Replicated Volumes are a preferred choice for Stateful workloads like Percona/MySQL, Jira, GitLab, etc. 

Depending on the type of storage attached to your Kubernetes worker nodes and application performance requirements, you can select from Jiva, cStor or Mayastor. 

### Replicated Volume Quickstart Guides

Installing OpenEBS in your cluster is as simple as running a few `kubectl` or `helm` commands. Here are the list of our Quickstart guides with detailed instructions for each storage engine.

- [Mayastor](/docs/next/ugmayastor.html)
- [cStor](https://github.com/openebs/cstor-operators/blob/master/docs/quick.md)
- [Jiva](https://github.com/openebs/jiva-operator)

## Selecting the right storage engine

See the following table for recommendation on which engine is right for your application depending on the application requirements and storage available on your Kubernetes nodes. 

| Application requirements   | Storage Type | OpenEBS Volumes
|--- |--- |--- 
| Low Latency, High Availability, Synchronous replication, Snapshots, Clones, Thin provisioning | SSDs/Cloud Volumes | [OpenEBS Mayastor](/docs/next/ugmayastor.html)
| High Availability, Synchronous replication, Snapshots, Clones, Thin provisioning | Disks/SSDs/Cloud Volumes | <a href="https://github.com/openebs/cstor-operators" target="_blank">OpenEBS cStor</a>
| High Availability, Synchronous replication, Thin provisioning | hostpath or external mounted storage | [OpenEBS Jiva](/docs/next/jivaguide.html)
| Low latency, Local PV | hostpath or external mounted storage | [Dynamic Local PV - Hostpath](/docs/next/uglocalpv-hostpath.html), <a href="https://github.com/openebs/rawfile-localpv" target="_blank">Dynamic Local PV - Rawfile</a>
| Low latency, Local PV | Disks/SSDs/Cloud Volumes | [Dynamic Local PV - Device](/docs/next/uglocalpv-device.html)
| Low latency, Local PV, Snapshots, Clones | Disks/SSDs/Cloud Volumes | <a href="https://github.com/openebs/zfs-localpv" target="_blank">OpenEBS Dynamic Local PV - ZFS </a>, <a href="https://github.com/openebs/lvm-localpv" target="_blank">OpenEBS Dynamic Local PV - LVM </a>


## Community Support via Slack

OpenEBS has a vibrant community that can help you get started. If you have further question and want to learn more about OpenEBS, please join [OpenEBS community on Kubernetes Slack](https://kubernetes.slack.com). If you are already signed up, head to our discussions at [#openebs](https://kubernetes.slack.com/messages/openebs/) channel. 

<br>

## See Also:

### [Use cases and Examples](/docs/next/usecases.html)

### [Container Attached Storage (CAS)](/docs/next/cas.html)

### [OpenEBS Architecture](/docs/next/architecture.html)

### [OpenEBS Local PV](/docs/next/localpv.html)

### [OpenEBS Mayastor](/docs/next/mayastor.html)

<br>
<br>
<hr>

